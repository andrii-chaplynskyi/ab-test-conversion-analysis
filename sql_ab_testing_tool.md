`# AB Testing - SQL for analysis`  
`/*`  
 `country,`  
 `device,`  
 `continent,`  
 `channe,`  
 `test,`  
 `test_group,`  
 `event_name, - session with orders - event name - session - new accounts`  
 `value`  
`*/`

`with session_info as (`  
 `SELECT`  
   `s.date,`  
   `s.ga_session_id,`  
   `sp.country,`  
   `sp.device,`  
   `sp.continent,`  
   `sp.channel,`  
   `ab.test,`  
   `ab.test_group`  
 ``FROM `DA.ab_test` ab``  
 ``JOIN `DA.session` s ON ab.ga_session_id = s.ga_session_id``  
 ``JOIN `DA.session_params` sp ON sp.ga_session_id = ab.ga_session_id``  
`),`  
`session_with_order AS (`  
`SELECT`  
   `session_info.date,`  
   `session_info.country,`  
   `session_info.device,`  
   `session_info.continent,`  
   `session_info.channel,`  
   `session_info.test,`  
   `session_info.test_group,`  
   `COUNT(DISTINCT o.ga_session_id) AS session_with_order`  
``FROM `DA.order` o``  
`JOIN session_info ON o.ga_session_id = session_info.ga_session_id`  
`GROUP BY`  
   `session_info.date,`  
   `session_info.country,`  
   `session_info.device,`  
   `session_info.continent,`  
   `session_info.channel,`  
   `session_info.test,`  
   `session_info.test_group`  
`),`  
`events AS (`  
`SELECT`  
 `session_info.date,`  
 `session_info.country,`  
 `session_info.device,`  
 `session_info.continent,`  
 `session_info.channel,`  
 `session_info.test,`  
 `session_info.test_group,`  
 `ep.event_name,`  
 `COUNT(ep.ga_session_id) AS event_cnt`  
``FROM `DA.event_params`ep``  
`JOIN session_info ON ep.ga_session_id = session_info.ga_session_id`  
`GROUP BY`  
 `session_info.date,`  
 `session_info.country,`  
 `session_info.device,`  
 `session_info.continent,`  
 `session_info.channel,`  
 `session_info.test,`  
 `session_info.test_group,`  
 `ep.event_name`  
`),`  
`session AS (`  
`SELECT`  
 `session_info.date,`  
 `session_info.country,`  
 `session_info.device,`  
 `session_info.continent,`  
 `session_info.channel,`  
 `session_info.test,`  
 `session_info.test_group,`  
 `COUNT(DISTINCT session_info.ga_session_id) AS session_cnt`  
`FROM session_info`  
`GROUP BY`  
 `session_info.date,`  
 `session_info.country,`  
 `session_info.device,`  
 `session_info.continent,`  
 `session_info.channel,`  
 `session_info.test,`  
 `session_info.test_group`  
`),`  
`account AS (`  
`SELECT`  
 `session_info.date,`  
 `session_info.country,`  
 `session_info.device,`  
 `session_info.continent,`  
 `session_info.channel,`  
 `session_info.test,`  
 `session_info.test_group,`  
 `COUNT(DISTINCT acs.ga_session_id) AS new_account_cnt`  
``FROM `DA.account_session` as acs``  
`JOIN session_info ON acs.ga_session_id = session_info.ga_session_id`  
`GROUP BY session_info.date,`  
 `session_info.country,`  
 `session_info.device,`  
 `session_info.continent,`  
 `session_info.channel,`  
 `session_info.test,`  
 `session_info.test_group`  
`)`

`SELECT`  
   `session_with_order.date,`  
   `session_with_order.country,`  
   `session_with_order.device,`  
   `session_with_order.continent,`  
   `session_with_order.channel,`  
   `session_with_order.test,`  
   `session_with_order.test_group,`  
   `'session with order' AS event_name,`  
   `session_with_order.session_with_order AS value`  
`FROM session_with_order`  
`UNION ALL`  
`SELECT`  
   `events.date,`  
   `events.country,`  
   `events.device,`  
   `events.continent,`  
   `events.channel,`  
   `events.test,`  
   `events.test_group,`  
   `event_name,`  
   `event_cnt AS value`  
`FROM events`  
`UNION ALL`  
`SELECT`  
   `session.date,`  
   `session.country,`  
   `session.device,`  
   `session.continent,`  
   `session.channel,`  
   `session.test,`  
   `session.test_group,`  
   `'session' AS event_name,`  
   `session_cnt AS value`  
`FROM session`  
`UNION ALL`  
`SELECT`  
   `account.date,`  
   `account.country,`  
   `account.device,`  
   `account.continent,`  
   `account.channel,`  
   `account.test,`  
   `account.test_group,`  
   `'new account' AS event_name,`  
   `account.new_account_cnt AS value`  
`FROM account`

https://public.tableau.com/app/profile/andrii.chaplynskyi/viz/ABTESTHW/ABtest?publish=yes