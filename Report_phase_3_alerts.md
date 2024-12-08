# Report_phase_3_alerts

ZAP by [Checkmarx](https://checkmarx.com/).


## Summary of Alerts

| Niveau de risque | Number of Alerts |
| --- | --- |
| Haut | 0 |
| Moyen | 1 |
| Faible | 1 |
| Pour information | 6 |




## Alertes

| Nom | Niveau de risque | Number of Instances |
| --- | --- | --- |
| Erreur de format de chaîne | Moyen | 1 |
| Faiblesse Cross Site Scripting (stocké dans la réponse JSON) | Faible | 2 |
| Authentication Request Identified | Pour information | 1 |
| Cookie faiblement couplé | Pour information | 2 |
| Information Disclosure - Suspicious Comments | Pour information | 1 |
| Modern Web Application | Pour information | 1 |
| Session Management Response Identified | Pour information | 2 |
| User Agent Fuzzer | Pour information | 108 |




## Alert Detail



### [ Erreur de format de chaîne ](https://www.zaproxy.org/docs/alerts/30002/)



##### Moyen (Moyen)

### Description

A Format String error occurs when the submitted data of an input string is evaluated as a command by the application.

* URL: http://localhost:8000/resources
  * Méthode: `POST`
  * Parameter: `resource_name`
  * Attaquer: `ZAP %1!s%2!s%3!s%4!s%5!s%6!s%7!s%8!s%9!s%10!s%11!s%12!s%13!s%14!s%15!s%16!s%17!s%18!s%19!s%20!s%21!n%22!n%23!n%24!n%25!n%26!n%27!n%28!n%29!n%30!n%31!n%32!n%33!n%34!n%35!n%36!n%37!n%38!n%39!n%40!n
`
  * Evidence: ``
  * Other Info: `Potential Format String Error. The script closed the connection on a Microsoft format string error.`

Instances: 1

### Solution

Rewrite the background program using proper deletion of bad character strings. This will require a recompile of the background executable.

### Reference


* [ https://owasp.org/www-community/attacks/Format_string_attack ](https://owasp.org/www-community/attacks/Format_string_attack)


#### CWE Id: [ 134 ](https://cwe.mitre.org/data/definitions/134.html)


#### WASC Id: 6

#### Source ID: 1

### [ Faiblesse Cross Site Scripting (stocké dans la réponse JSON) ](https://www.zaproxy.org/docs/alerts/40014/)



##### Faible (Faible)

### Description

Une attaque XSS a été trouvée dans la réponse JSON, cela peut rendre vulnérable les composants qui consommeront ce contenu s'ils ne gèrent pas ces données (réponse JSON) de manière appropriée.

* URL: http://localhost:8000/resourcesList
  * Méthode: `GET`
  * Parameter: `resource_description`
  * Attaquer: `<script>alert(1);</script>`
  * Evidence: ``
  * Other Info: `Raised with LOW confidence as the Content-Type is not HTML.`
* URL: http://localhost:8000/resourcesList
  * Méthode: `GET`
  * Parameter: `resource_name`
  * Attaquer: `<script>alert(1);</script>`
  * Evidence: ``
  * Other Info: `Raised with LOW confidence as the Content-Type is not HTML.`

Instances: 2

### Solution

Phase: Architecture et Design
Utilisez une librairie ou un framework approuvé qui ne permet pas cette vulnérabilité, ou qui contient des fonctionnalités permettant d'éviter plus facilement cette vulnérabilité.
Quelques exemples de bibliothèques et de frameworks facilitant l'encodage approprié des pages internet sont: la bibliothèque Microsoft Anti-XSS, le module d'encodage OWASP ESAPI, et Apache Wicket.

Phases: Implémentation; Architecture et Design
Tenez compte du contexte dans lequel vos données seront utilisées et de l'encodage qui sera attendu. Ceci est particulièrement important lors de la transmission de données entre les différents composants, ou lors de la génération de sorties qui peuvent contenir plusieurs encodages en même temps, comme des pages internet ou des messages électroniques multi-parties. Étudiez tous les protocoles de communication et les représentations de données attendus pour déterminer les stratégies de codage requis.
Pour toutes les données qui seront affichées sur une autre page web, en particulier toutes les données qui ont été reçues de l'extérieur, utilisez l'encodage approprié pour tous les caractères non alphanumériques.
Consultez le XSS Prevention Cheat Sheet pour plus de détails sur les types d'encodage et d'échappement dont vous pouvez avoir besoin.

Phase : Architecture et Design
Pour tous les contrôles de sécurité effectués du côté client, veillez à ce que ces contrôles soient réitérés du côté serveur également, afin d'éviter la faille CWE-602. Les agresseurs peuvent contourner les contrôles côté client en modifiant les valeurs après que ces contrôles aient été effectués, ou en changeant le client afin d'en retirer complètement les contrôles. Ensuite, ces valeurs modifiées seraient soumises au serveur.

Le cas échéant, utilisez des mécanismes structurés appliquant automatiquement la séparation entre le code et les données. Ces mécanismes peuvent être en mesure de fournir automatiquement la présentation, le codage et la validation adéquat, au lieu de se fier au développeur pour réaliser ces fonctionnalités pour chaque champ de sortie généré.

Phase: Implémentation
Pour chaque page internet générée, utilisez et spécifiez un encodage de chaînes de caractères comme ISO-8859-1 ou UTF-8. Quand aucun codage n'est spécifié, le navigateur web tente de deviner quel encodage est effectivement utilisé dans la page web et peut choisir un codage erroné. Ceci peut pousser le navigateur internet à traiter spécialement certaines séquences, exposant ainsi le client à de subtiles attaques XSS. Voir CWE-116 pour des mesures de réduction de risque plus liées à l'encodage / l'échappement.

Pour aider à réduire les attaques XSS contre le cookie de session de l'utilisateur, fixez la valeur du cookie à HttpOnly. Dans les navigateurs prennant en charge la fonctionnalité HttpOnly (telles que les versions les plus récentes d'Internet Explorer et de Firefox), cet attribut peut éviter que le cookie de session de l'utilisateur soit accessible à des scripts malveillants utilisant document.cookie côté client. Ce n'est toutefois pas une solution complète, car HttpOnly n'est pas supporté par tous les navigateurs. Plus important encore, XMLHTTPRequest et d'autres puissantes  technologies de navigateur fournissent un accès en lecture aux en-têtes HTTP, y compris à l'en-tête Set-Cookie, dans laquelle la balise HttpOnly est définie.

Supposez que toutes les entrées sont malveillantes. Use an "accept known good" input validation strategy, i.e., use an allow list of acceptable inputs that strictly conform to specifications. Rejetez toute entrée ne se conformant pas strictement aux spécifications, ou transformez-la en une valeur qui soit conforme. Do not rely exclusively on looking for malicious or malformed inputs (i.e., do not rely on a deny list). However, deny lists can be useful for detecting potential attacks or determining which inputs are so malformed that they should be rejected outright.

Lorsque vous effectuez une validation d'entrée, considérez toutes les propriétés potentiellement pertinentes, comme la longueur, le type d'entrée, la gamme complète des valeurs acceptables, les entrées manquantes ou défectueuses, la syntaxe, la cohérence dans des domaines connexes et la conformité aux règles métier. As an example of business rule logic, "boat" may be syntactically valid because it only contains alphanumeric characters, but it is not valid if you are expecting colors such as "red" or "blue."

Ensure that you perform input validation at well-defined interfaces within the application. Ceci aidera à protéger l'application, même si un composant est réutilisé ou déplacé ailleurs.
	

### Reference


* [ https://owasp.org/www-community/attacks/xss/ ](https://owasp.org/www-community/attacks/xss/)
* [ https://cwe.mitre.org/data/definitions/79.html ](https://cwe.mitre.org/data/definitions/79.html)


#### CWE Id: [ 79 ](https://cwe.mitre.org/data/definitions/79.html)


#### WASC Id: 8

#### Source ID: 1

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
userValue=azerty@test.com
passwordParam=password
referer=https://localhost:8000/login`

Instances: 1

### Solution

This is an informational alert rather than a vulnerability and so there is nothing to fix.

### Reference


* [ https://www.zaproxy.org/docs/desktop/addons/authentication-helper/auth-req-id/ ](https://www.zaproxy.org/docs/desktop/addons/authentication-helper/auth-req-id/)



#### Source ID: 3

### [ Cookie faiblement couplé ](https://www.zaproxy.org/docs/alerts/90033/)



##### Pour information (Faible)

### Description

Les cookies peuvent être limités par le domaine ou le chemin d'accès. Cette vérification ne concerne que le périmètre de domaine. Le périmètre de domaine appliqué à un cookie détermine quels domaines peuvent accéder à ce cookie. Par exemple, un cookie peut être limité strictement à un sous-domaine, p.ex. www.nottrusted.com, ou faiblement limité à un domaine parent, p.ex. nottrusted.com. Dans ce dernier cas, n'importe quel sous-domaine de nottrusted.com peut accéder au cookie. Des cookies faiblement limités sont fréquents dans les méga-applications, comme google.com et live.com.

* URL: http://localhost:8000/logout
  * Méthode: `GET`
  * Parameter: ``
  * Attaquer: ``
  * Evidence: ``
  * Other Info: `The origin domain used for comparison was:
localhost
session_id="";$Path="/";$Domain="localhost"

`
* URL: http://localhost:8000/login
  * Méthode: `POST`
  * Parameter: ``
  * Attaquer: ``
  * Evidence: ``
  * Other Info: `The origin domain used for comparison was:
localhost
session_id=1013126d-586d-431f-ae16-075723c33f33

`

Instances: 2

### Solution

Toujours limiter les cookies à un FQDN (Fully Qualified Domain Name).

### Reference


* [ https://tools.ietf.org/html/rfc6265#section-4.1 ](https://tools.ietf.org/html/rfc6265#section-4.1)
* [ https://owasp.org/www-project-web-security-testing-guide/v41/4-Web_Application_Security_Testing/06-Session_Management_Testing/02-Testing_for_Cookies_Attributes.html ](https://owasp.org/www-project-web-security-testing-guide/v41/4-Web_Application_Security_Testing/06-Session_Management_Testing/02-Testing_for_Cookies_Attributes.html)
* [ https://code.google.com/p/browsersec/wiki/Part2#Same-origin_policy_for_cookies ](https://code.google.com/p/browsersec/wiki/Part2#Same-origin_policy_for_cookies)


#### CWE Id: [ 565 ](https://cwe.mitre.org/data/definitions/565.html)


#### WASC Id: 15

#### Source ID: 3

### [ Information Disclosure - Suspicious Comments ](https://www.zaproxy.org/docs/alerts/10027/)



##### Pour information (Moyen)

### Description

The response appears to contain suspicious comments which may help an attacker. Note: Matches made within script blocks or files are against the entire content not only comments.

* URL: http://localhost:8000/reservation
  * Méthode: `GET`
  * Parameter: ``
  * Attaquer: ``
  * Evidence: `username`
  * Other Info: `The following pattern was used: \bUSERNAME\b and was detected in the element starting with: "<!-- Reserver username (pre-filled) -->", see evidence field for the suspicious comment/snippet.`

Instances: 1

### Solution

Remove all comments that return information that may help an attacker and fix any underlying problems they refer to.

### Reference



#### CWE Id: [ 200 ](https://cwe.mitre.org/data/definitions/200.html)


#### WASC Id: 13

#### Source ID: 3

### [ Modern Web Application ](https://www.zaproxy.org/docs/alerts/10109/)



##### Pour information (Moyen)

### Description

The application appears to be a modern web application. If you need to explore it automatically then the Ajax Spider may well be more effective than the standard one.

* URL: http://localhost:8000/reservation
  * Méthode: `GET`
  * Parameter: ``
  * Attaquer: ``
  * Evidence: `<script src="/static/reservations.js"></script>`
  * Other Info: `No links have been found while there are scripts, which is an indication that this is a modern web application.`

Instances: 1

### Solution

This is an informational alert and so no changes are required.

### Reference




#### Source ID: 3

### [ Session Management Response Identified ](https://www.zaproxy.org/docs/alerts/10112/)



##### Pour information (Haut)

### Description

The given response has been identified as containing a session management token. The 'Other Info' field contains a set of header tokens that can be used in the Header Based Session Management Method. If the request is in a context which has a Session Management Method set to "Auto-Detect" then this rule will change the session management to use the tokens identified.

* URL: http://localhost:8000/login
  * Méthode: `POST`
  * Parameter: `session_id`
  * Attaquer: ``
  * Evidence: `1013126d-586d-431f-ae16-075723c33f33`
  * Other Info: `
cookie:session_id`
* URL: http://localhost:8000/login
  * Méthode: `GET`
  * Parameter: `session_id`
  * Attaquer: ``
  * Evidence: `1013126d-586d-431f-ae16-075723c33f33`
  * Other Info: `
cookie:session_id`

Instances: 2

### Solution

This is an informational alert rather than a vulnerability and so there is nothing to fix.

### Reference


* [ https://www.zaproxy.org/docs/desktop/addons/authentication-helper/session-mgmt-id ](https://www.zaproxy.org/docs/desktop/addons/authentication-helper/session-mgmt-id)



#### Source ID: 3

### [ User Agent Fuzzer ](https://www.zaproxy.org/docs/alerts/10104/)



##### Pour information (Moyen)

### Description

Check for differences in response based on fuzzed User Agent (eg. mobile sites, access as a Search Engine Crawler). Compares the response statuscode and the hashcode of the response body with the original response.

* URL: http://localhost:8000
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 6.0)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 6.1)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (Windows NT 10.0; Trident/7.0; rv:11.0) like Gecko`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3739.0 Safari/537.36 Edg/75.0.109.0`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:93.0) Gecko/20100101 Firefox/91.0`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (compatible; Yahoo! Slurp; http://help.yahoo.com/help/us/ysearch/slurp)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (iPhone; CPU iPhone OS 8_0_2 like Mac OS X) AppleWebKit/600.1.4 (KHTML, like Gecko) Version/8.0 Mobile/12A366 Safari/600.1.4`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (iPhone; U; CPU iPhone OS 3_0 like Mac OS X; en-us) AppleWebKit/528.18 (KHTML, like Gecko) Version/4.0 Mobile/7A341 Safari/528.16`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `msnbot/1.1 (+http://search.msn.com/msnbot.htm)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 6.0)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 6.1)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (Windows NT 10.0; Trident/7.0; rv:11.0) like Gecko`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3739.0 Safari/537.36 Edg/75.0.109.0`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:93.0) Gecko/20100101 Firefox/91.0`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (compatible; Yahoo! Slurp; http://help.yahoo.com/help/us/ysearch/slurp)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (iPhone; CPU iPhone OS 8_0_2 like Mac OS X) AppleWebKit/600.1.4 (KHTML, like Gecko) Version/8.0 Mobile/12A366 Safari/600.1.4`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (iPhone; U; CPU iPhone OS 3_0 like Mac OS X; en-us) AppleWebKit/528.18 (KHTML, like Gecko) Version/4.0 Mobile/7A341 Safari/528.16`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `msnbot/1.1 (+http://search.msn.com/msnbot.htm)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/logout
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/logout
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 6.0)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/logout
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 6.1)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/logout
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (Windows NT 10.0; Trident/7.0; rv:11.0) like Gecko`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/logout
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3739.0 Safari/537.36 Edg/75.0.109.0`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/logout
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/logout
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:93.0) Gecko/20100101 Firefox/91.0`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/logout
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/logout
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (compatible; Yahoo! Slurp; http://help.yahoo.com/help/us/ysearch/slurp)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/logout
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (iPhone; CPU iPhone OS 8_0_2 like Mac OS X) AppleWebKit/600.1.4 (KHTML, like Gecko) Version/8.0 Mobile/12A366 Safari/600.1.4`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/logout
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (iPhone; U; CPU iPhone OS 3_0 like Mac OS X; en-us) AppleWebKit/528.18 (KHTML, like Gecko) Version/4.0 Mobile/7A341 Safari/528.16`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/logout
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `msnbot/1.1 (+http://search.msn.com/msnbot.htm)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/reservation
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/reservation
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 6.0)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/reservation
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 6.1)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/reservation
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (Windows NT 10.0; Trident/7.0; rv:11.0) like Gecko`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/reservation
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3739.0 Safari/537.36 Edg/75.0.109.0`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/reservation
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/reservation
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:93.0) Gecko/20100101 Firefox/91.0`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/reservation
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/reservation
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (compatible; Yahoo! Slurp; http://help.yahoo.com/help/us/ysearch/slurp)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/reservation
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (iPhone; CPU iPhone OS 8_0_2 like Mac OS X) AppleWebKit/600.1.4 (KHTML, like Gecko) Version/8.0 Mobile/12A366 Safari/600.1.4`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/reservation
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (iPhone; U; CPU iPhone OS 3_0 like Mac OS X; en-us) AppleWebKit/528.18 (KHTML, like Gecko) Version/4.0 Mobile/7A341 Safari/528.16`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/reservation
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `msnbot/1.1 (+http://search.msn.com/msnbot.htm)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/resourcesList
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/resourcesList
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 6.0)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/resourcesList
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 6.1)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/resourcesList
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (Windows NT 10.0; Trident/7.0; rv:11.0) like Gecko`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/resourcesList
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3739.0 Safari/537.36 Edg/75.0.109.0`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/resourcesList
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/resourcesList
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:93.0) Gecko/20100101 Firefox/91.0`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/resourcesList
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/resourcesList
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (compatible; Yahoo! Slurp; http://help.yahoo.com/help/us/ysearch/slurp)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/resourcesList
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (iPhone; CPU iPhone OS 8_0_2 like Mac OS X) AppleWebKit/600.1.4 (KHTML, like Gecko) Version/8.0 Mobile/12A366 Safari/600.1.4`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/resourcesList
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (iPhone; U; CPU iPhone OS 3_0 like Mac OS X; en-us) AppleWebKit/528.18 (KHTML, like Gecko) Version/4.0 Mobile/7A341 Safari/528.16`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/resourcesList
  * Méthode: `GET`
  * Parameter: `Header User-Agent`
  * Attaquer: `msnbot/1.1 (+http://search.msn.com/msnbot.htm)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/login
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/login
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 6.0)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/login
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 6.1)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/login
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (Windows NT 10.0; Trident/7.0; rv:11.0) like Gecko`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/login
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3739.0 Safari/537.36 Edg/75.0.109.0`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/login
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/login
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:93.0) Gecko/20100101 Firefox/91.0`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/login
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/login
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (compatible; Yahoo! Slurp; http://help.yahoo.com/help/us/ysearch/slurp)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/login
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (iPhone; CPU iPhone OS 8_0_2 like Mac OS X) AppleWebKit/600.1.4 (KHTML, like Gecko) Version/8.0 Mobile/12A366 Safari/600.1.4`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/login
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (iPhone; U; CPU iPhone OS 3_0 like Mac OS X; en-us) AppleWebKit/528.18 (KHTML, like Gecko) Version/4.0 Mobile/7A341 Safari/528.16`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/login
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `msnbot/1.1 (+http://search.msn.com/msnbot.htm)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/register
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/register
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 6.0)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/register
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 6.1)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/register
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (Windows NT 10.0; Trident/7.0; rv:11.0) like Gecko`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/register
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3739.0 Safari/537.36 Edg/75.0.109.0`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/register
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/register
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:93.0) Gecko/20100101 Firefox/91.0`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/register
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/register
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (compatible; Yahoo! Slurp; http://help.yahoo.com/help/us/ysearch/slurp)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/register
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (iPhone; CPU iPhone OS 8_0_2 like Mac OS X) AppleWebKit/600.1.4 (KHTML, like Gecko) Version/8.0 Mobile/12A366 Safari/600.1.4`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/register
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (iPhone; U; CPU iPhone OS 3_0 like Mac OS X; en-us) AppleWebKit/528.18 (KHTML, like Gecko) Version/4.0 Mobile/7A341 Safari/528.16`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/register
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `msnbot/1.1 (+http://search.msn.com/msnbot.htm)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/reservation
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/reservation
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 6.0)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/reservation
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 6.1)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/reservation
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (Windows NT 10.0; Trident/7.0; rv:11.0) like Gecko`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/reservation
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3739.0 Safari/537.36 Edg/75.0.109.0`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/reservation
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/reservation
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:93.0) Gecko/20100101 Firefox/91.0`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/reservation
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/reservation
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (compatible; Yahoo! Slurp; http://help.yahoo.com/help/us/ysearch/slurp)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/reservation
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (iPhone; CPU iPhone OS 8_0_2 like Mac OS X) AppleWebKit/600.1.4 (KHTML, like Gecko) Version/8.0 Mobile/12A366 Safari/600.1.4`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/reservation
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (iPhone; U; CPU iPhone OS 3_0 like Mac OS X; en-us) AppleWebKit/528.18 (KHTML, like Gecko) Version/4.0 Mobile/7A341 Safari/528.16`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/reservation
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `msnbot/1.1 (+http://search.msn.com/msnbot.htm)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/resources
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/resources
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 6.0)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/resources
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 6.1)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/resources
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (Windows NT 10.0; Trident/7.0; rv:11.0) like Gecko`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/resources
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3739.0 Safari/537.36 Edg/75.0.109.0`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/resources
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/resources
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:93.0) Gecko/20100101 Firefox/91.0`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/resources
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/resources
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (compatible; Yahoo! Slurp; http://help.yahoo.com/help/us/ysearch/slurp)`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/resources
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (iPhone; CPU iPhone OS 8_0_2 like Mac OS X) AppleWebKit/600.1.4 (KHTML, like Gecko) Version/8.0 Mobile/12A366 Safari/600.1.4`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/resources
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `Mozilla/5.0 (iPhone; U; CPU iPhone OS 3_0 like Mac OS X; en-us) AppleWebKit/528.18 (KHTML, like Gecko) Version/4.0 Mobile/7A341 Safari/528.16`
  * Evidence: ``
  * Other Info: ``
* URL: http://localhost:8000/resources
  * Méthode: `POST`
  * Parameter: `Header User-Agent`
  * Attaquer: `msnbot/1.1 (+http://search.msn.com/msnbot.htm)`
  * Evidence: ``
  * Other Info: ``

Instances: 108

### Solution



### Reference


* [ https://owasp.org/wstg ](https://owasp.org/wstg)



#### Source ID: 1


