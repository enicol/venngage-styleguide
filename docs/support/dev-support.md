# Dev Support Wiki

 # Dev Support Wiki


## [Outline](#outline)
1. [Downgrades](#downgrades)
1. [Upgrades](#upgrades)
1. [Cancel Subscription](#cancel-subscription)
1. [Downgrade Site License](#downgrade-site-license)
1. [Restore Infographic Widgets](#restore-infographic-widgets)

### USER TYPES
1. *Business*: team_id != null
2. *Premium* || *Premium Guest* (w teamID)
1: *Free* (and make sure team_id = NULL)

## Downgrades

### Business to Premium account
---
1. Find the user by email:

```sql
SELECT * from users where email like 'xxx@xxxxx.com';
```

2. Find the user's subscription by user id:

```sql
SELECT * from subscriptions where user_id = 12345
```

3. Verify the subscription in stripe dashboard:

    https://dashboard.stripe.com/customers/cus_xxxxxx

4. Downgrade user subscription via stripe dashboard, ensure prorate fee checkbox off

5. Update subscription field in subscription tables

```sql
UPDATE vizmvp.subscriptions SET subscription='Infographic_Premium_1' WHERE id='1074742';
```

6. Find the user's team in teams table

```sql
SELECT * from teams where owner_id ='12345';
```

7. Invalidate the team record

```sql
UPDATE vizmvp.teams SET invalid='1' WHERE id='xxxx';
```

8. Invalidate the team members belong to the team:

```sql
UPDATE vizmvp.team_members SET invalid='1' WHERE id='4891';
```

9. Invalidate the brand record from brands table

10. Update user record in users table

```sql
UPDATE vizmvp.users SET user_type='2', team_id=NULL WHERE id='12345';
```

11. Invalidate all the team shares in team_shares table



## Upgrade existing premium user to Business
---
1. Find the user by email
2. Find the user's subscription by user_id
3. If stripe already has the business subscription, update our subscriptions table:

4. Create team for this user:*
    ```SQL
    INSERT INTO vizmvp.teams (owner_id, team_name, created_time, invalid, base_members, max_members, member_price, type) VALUES ('1626998', 'Business Team', NOW(), '0', '1', '1', '468', '3');
    ```

 5. Add user as team member in team_members table:
    ```SQL
    INSERT INTO vizmvp.team_members (user_id, team_id, member_email, invalid, created_time) VALUES ('19306', 'xxx', 'xxxx@venngage.com', '0', NOW());
    ```

6. Update user in users table:

    ```sql
    UPDATE vizmvp.users SET user_type='1', team_id='8064' WHERE id='1180021';
    ```

New Pricing:

base_members= 1, max_members = 1, member_price = 468(yearly), 129(quarterly), 49(monthly)

Additional members charge:

> (max_members - base_members) * member_price

**max_members always >= base_members, member_price in USD**

> If max_members === base_members, they will not be charged for additional team members

_PREMIUM_                                                            _BUSINESS_
* Infographic_Premium_1           $19                     Business_Plan_1         $49
* Infographic_Premium_3           $49                     Business_Plan_3         $129
* Infographic_Premium_12        $190                   Business_Plan_12       $468

Old Pricing:
::base_members = 2, max_members = 2, member_price = 180 (yearly), 49(quarterly), 19(monthly)::



### Refund premium user & Downgrade immediately
---
1. Find user by email:

    ```sql
    SELECT * from users where email= 'abc@xxx.com'
    ```

2. Find subscription by user_id:

    ```sql
    SELECT * from subscriptions where user_id = 1234
    ```

3. Find user stripe account in stripe dashboard - refund user
4. If user has active subscription, cancelled immediately
5. Make sure user's subscription is cancelled in subscriptions table
6. Make sure user_type is 1 in user tables
7. Make sure all user private_shares become invalid


### Downgrade Premium Guest
---
1. Change user_type to 1 in users table:

`UPDATE vizmvp.users SET user_type='1' WHERE id='1234';`

2. Invalid all user share in private_shares table:

`UPDATE private_shares SET invalid = 1 WHERE owner_id = 1234;`



### Upgrade from Free to Educational Yearly
---
1. Find user by email:
`SELECT * from users where email like 'abc@xxx.com'`

2. Check if the user has subscription:
`SELECT * from subscriptions where user_id = 1392424`

3. Create team for the user:

`INSERT INTO vizmvp.teams (owner_id, team_name, created_time, invalid, max_member, type) VALUES ('1392424', 'New Class', NOW());`

4. Add user as team member in team_members table:

`INSERT INTO vizmvp.team_members (user_id, team_id, member_email, invalid) VALUES ('1234', '5678', 'abc@xxx.com', 0);`

5. Update user in users table:
`UPDATE vizmvp.users SET team_id='5678' WHERE id='1234’;`

## Cancel Subscription
---
Update subscription & status field in subscription tables
`UPDATE vizmvp.subscriptions SET subscription='cancelled', status= 0 WHERE id='1420217';`

Update cancelled_at field in subscription tables
`UPDATE vizmvp.subscriptions SET cancelled_at= NOW() WHERE id='xxx';`


## Downgrade Site License
---
*Only if users don’t have a subscription*

1. Find all the premium guest users that have the domain 'test.com'
```SQL
SELECT
    u.id
FROM
    users u
WHERE
    u.email LIKE '%test.com'
        AND u.user_type = 2
        AND u.enabled = 1
```

2. Find the users that have subscriptions and domain with 'test.com'
```SQL
SELECT
    COUNT(id)
FROM
    subscriptions
WHERE
    user_id IN (SELECT
            u.id
        FROM
            users u
        WHERE
            u.email LIKE '%test.com'
                AND u.user_type = 2
                AND u.enabled = 1)
```
*Note: if some users have existing subscriptions do not proceed to next step and instead adjust query to exclude those with subscriptions*

3. Turn off all the private email shares for users with domain test.com
```SQL
SET SQL_SAFE_UPDATES = 0;

UPDATE `vizmvp`.`private_shares`
SET
    `invalid`='1', `unshared_at`='2017-09-11'
WHERE
    `invalid` = '0'
        AND `owner_id` IN (SELECT
            u.id
        FROM
            users u
        WHERE
            u.email LIKE '%test.com'
                AND u.user_type = 2
                AND u.enabled = 1);

SET SQL_SAFE_UPDATES = 1;
```

4. Downgrade those premium guest users to free
```SQL
SET SQL_SAFE_UPDATES = 0;

UPDATE users
SET
    user_type = '1'
WHERE
    email LIKE '%test.com' AND user_type = 2 AND enabled = 1;

SET SQL_SAFE_UPDATES = 1;
```

5. Invalid the site license. (note: assume id 1000 is ref to test.com)
`UPDATE vizmvp.site_licenses SET invalid='1' WHERE id='1000';`

Custom Export Link (brute force uses frontend server)
All you need to do is swap out the infographic_id and replace it in the url. Then `cmd + p` to print

https://infograph.venngage.com/exportgenerator/infograph_export/c84029cd-cd55-4ae5-98b7-1b36abb6d064?export_token=takeaguess


## Inspect specific widget_id in browser console
---
Select intended widget in editor
 `page.activeWidget.identifier` to return its *widget_id*



## Restore Infographic Widgets
---
1. If it’s a multipage infographic, navigate to the page and in the console type `pageId` to retrieve the page_id

2. Go to the DB and search for rows that match page_id in widget table

```sql
SELECT * FROM widget WHERE page_id='652205fa-63e8-43e4-b454-d32b771daac5'
```

3. Find at what time the `modified_at` column shows multiple records edited at exactly the same time. This is where the infographic likely got screwed up

4. Using that `modified_at` timestamp

```sql
SELECT * FROM widget WHERE page_id='**********' AND modified_at > '2018-03-16 16:06:00' ORDER BY modified_at
```

5. Then go back to the editor and in the console type `page.order` to check if the infographic page’s widget order is still intact

6. If it is, run the snippet called `widget_recover.js` or simply run this:
```js
var eResult = '';

page.order.forEach(v => {
	eResult += `"${v}",`
});

eResult = eResult.replace(/,$/, '');
copy(eResult);
```

7. Now use that list to search for the matching widget_id’s in the widget table

```sql
SELECT * FROM widget WHERE widget_id IN (list of all IDs)
```

## Custom Export
For for single page Infographics

Insert infographic pageId in blank
https://infograph.venngage.com/exportgenerator/infograph_export/_______________?export_token=takeaguess

ex. https://infograph.venngage.com/exportgenerator/infograph_export/815062d2-346b-477c-a243-c408613af982?export_token=takeaguess
