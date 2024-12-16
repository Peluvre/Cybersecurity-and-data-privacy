# Report_phase_3_alerts

ZAP by [Checkmarx](https://checkmarx.com/).


## Summary of Alerts

| Niveau de risque | Number of Alerts |
| --- | --- |
| Haut | 0 |
| Moyen | 0 |
| Faible | 0 |
| Pour information | 1 |




## Alertes

| Nom | Niveau de risque | Number of Instances |
| --- | --- | --- |
| Authentication Request Identified | Pour information | 1 |




## Alert Detail



### [ Authentication Request Identified ](https://www.zaproxy.org/docs/alerts/10111/)



##### Pour information (Haut)

### Description

The given request has been identified as an authentication request. The 'Other Info' field contains a set of key=value lines which identify any relevant fields. If the request is in a context which has an Authentication Method set to "Auto-Detect" then this rule will change the authentication to match the request identified.

* URL: http://localhost:8000/login
  * Méthode: `POST`
  * Parameter: `username`
  * Attaquer: ``
  * Evidence: `password`
  * Other Info: `userParam=username
userValue=foo-bar@example.com
passwordParam=password
referer=http://localhost:8000/login`

Instances: 1

### Solution

This is an informational alert rather than a vulnerability and so there is nothing to fix.

### Reference


* [ https://www.zaproxy.org/docs/desktop/addons/authentication-helper/auth-req-id/ ](https://www.zaproxy.org/docs/desktop/addons/authentication-helper/auth-req-id/)



#### Source ID: 3


