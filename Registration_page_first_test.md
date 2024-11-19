# ZAP by Checkmarx Scanning Report

ZAP by [Checkmarx](https://checkmarx.com/).


## Summary of Alerts

| Niveau de risque | Number of Alerts |
| --- | --- |
| Haut | 2 |
| Moyen | 2 |
| Faible | 2 |
| Pour information | 0 |




## Alertes

| Nom | Niveau de risque | Number of Instances |
| --- | --- | --- |
| Injection SQL | Haut | 1 |
| Traversée de chemin | Haut | 1 |
| Content Security Policy (CSP) Header Not Set | Moyen | 1 |
| Missing Anti-clickjacking Header | Moyen | 1 |
| Application Error Disclosure | Faible | 1 |
| X-Content-Type-Options Header Missing | Faible | 1 |




## Alert Detail



### [ Injection SQL ](https://www.zaproxy.org/docs/alerts/40018/)



##### Haut (Moyen)

### Description

SQL injection may be possible.

* URL: http://localhost:8000/register
  * Méthode: `POST`
  * Parameter: `username`
  * Attaquer: `ZAP AND 1=1 -- `
  * Evidence: ``
  * Other Info: `The page results were successfully manipulated using the boolean conditions [ZAP AND 1=1 -- ] and [ZAP AND 1=2 -- ]
The parameter value being modified was NOT stripped from the HTML output for the purposes of the comparison.
Data was returned for the original parameter.
The vulnerability was detected by successfully restricting the data originally returned, by manipulating the parameter.`

Instances: 1

### Solution

Do not trust client side input, even if there is client side validation in place.
In general, type check all data on the server side.
If the application uses JDBC, use PreparedStatement or CallableStatement, with parameters passed by '?'
If the application uses ASP, use ADO Command Objects with strong type checking and parameterized queries.
If database Stored Procedures can be used, use them.
Do *not* concatenate strings into queries in the stored procedure, or use 'exec', 'exec immediate', or equivalent functionality!
Do not create dynamic SQL queries using simple string concatenation.
Escape all data received from the client.
Apply an 'allow list' of allowed characters, or a 'deny list' of disallowed characters in user input.
Apply the principle of least privilege by using the least privileged database user possible.
In particular, avoid using the 'sa' or 'db-owner' database users. This does not eliminate SQL injection, but minimizes its impact.
Grant the minimum database access that is necessary for the application.

### Reference


* [ https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html ](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)


#### CWE Id: [ 89 ](https://cwe.mitre.org/data/definitions/89.html)


#### WASC Id: 19

#### Source ID: 1

### [ Traversée de chemin ](https://www.zaproxy.org/docs/alerts/6/)



##### Haut (Faible)

### Description

La technique d'attaque par traversée de chemin d'accès (path traversal) permet à un pirate d'accéder aux fichiers, répertoires et commandes résidant potentiellement en dehors du répertoire racine des documents internet. Un agresseur peut manipuler une URL de telle sorte que le site internet exécutera ou révélera le contenu de fichiers arbitraires situés n'importe où sur le serveur internet. Tout dispositif qui expose une interface HTTP est potentiellement vulnérable à la traversée de chemin.

La plupart des sites internet restreignent l'accès de l'utilisateur à une partie spécifique du système de fichiers, généralement appelée la "racine des documents internet" ou le répertoire "Racine CGI". Ces répertoires contiennent les fichiers destinés à l'accès des utilisateurs et les exécutables nécessaires au fonctionnement des applications internet. Pour accéder aux fichiers ou exécuter des commandes situées n'importe où dans le système de fichiers, les attaques par traversée de chemin utiliseront les capacités de séquences de caractères spéciaux.

L'attaque par traversée de chemin la plus élémentaire utilise la séquence de caractères spéciaux "../ " pour modifier l'emplacement de la ressource demandée dans l'URL. Bien que la plupart des serveurs internet les plus populaires empêcheront de sortir de la racine des documents internet par cette technique, des encodages alternatifs de la séquence "../" peuvent aider à contourner les filtres de sécurité. Ces variations de la méthode incluent le codage Unicode valide et non valide ("..%u2216" ou "..%c0%af ») de la barre oblique, des barres obliques inverses ("..\") sur des serveurs Windows, des caractères en codage d'URL ("%2e%2e%2f"), et le double codage d'URL ("..%255c") de la barre oblique.

Et même si le serveur internet limite correctement les tentatives de traversée de chemin d'accès dans le chemin d'URL, une application internet peut encore être elle-même vulnérable en raison d'un mauvais traitement des entrées fournies par l'utilisateur. Il s'agit d'un problème commun des applications internet qui utilisent des mécanismes de modèle ou qui chargent du texte statique à partir de fichiers. Dans des variantes de cette attaque, la valeur du paramètre de l'URL originale est remplacée par le nom de fichier de l'un des scripts dynamiques de l'application internet. En conséquence, les résultats peuvent révéler le code source, parce que le fichier est interprété comme du texte plutôt que comme un script exécutable. These techniques often employ additional special characters such as the dot (".") to reveal the listing of the current working directory, or "%00" NULL characters in order to bypass rudimentary file extension checks.

* URL: http://localhost:8000/register
  * Méthode: `POST`
  * Parameter: `username`
  * Attaquer: `register`
  * Evidence: ``
  * Other Info: ``

Instances: 1

### Solution

Supposez que toutes les entrées sont malveillantes. Use an "accept known good" input validation strategy, i.e., use an allow list of acceptable inputs that strictly conform to specifications. Rejetez toute entrée ne se conformant pas strictement aux spécifications, ou transformez-la en une valeur qui soit conforme. Do not rely exclusively on looking for malicious or malformed inputs (i.e., do not rely on a deny list). However, deny lists can be useful for detecting potential attacks or determining which inputs are so malformed that they should be rejected outright.

Lorsque vous effectuez une validation d'entrée, considérez toutes les propriétés potentiellement pertinentes, comme la longueur, le type d'entrée, la gamme complète des valeurs acceptables, les entrées manquantes ou défectueuses, la syntaxe, la cohérence dans des domaines connexes et la conformité aux règles métier. As an example of business rule logic, "boat" may be syntactically valid because it only contains alphanumeric characters, but it is not valid if you are expecting colors such as "red" or "blue."

For filenames, use stringent allow lists that limit the character set to be used. Si possible, ne permettez qu'un seul caractère "." dans le nom de fichier pour éviter les failles et excluez les séparateurs de répertoire comme "/". Use an allow list of allowable file extensions.

Avertissement: si vous essayez de nettoyer vos données, faites-le alors de manière à ce que le résultat final ne soit pas sous une forme qui puisse être dangereuse. Un mécanisme d'assainissement peut supprimer des caractères tels que '.' et ';', qui peuvent être nécessaires pour certains exploits. Un agresseur peut essayer de tromper le mécanisme d'assainissement en transformant les données sous une forme dangereuse. Supposons que le pirate injecte un '.' à l'intérieur d'un nom de fichier (par exemple "sensi.tiveFile"), et le mécanisme d'assainissement supprimera le caractère, résultant en un nom de fichier valide, "sensitiveFile". Si les données d'entrée sont maintenant supposées sûres, alors le fichier peut être compromis. 

Les entrées devraient être décodées et rendues canoniques selon la représentation interne actuelle de l'application avant d'en effectuer la validation. Assurez-vous que votre application ne décode pas la même entrée deux fois. Such errors could be used to bypass allow list schemes by introducing dangerous inputs after they have been checked.

Utilisez une fonction prédéfinie de canonisation du chemin (par exemple la fonction realpath() en C), qui produit la version canonique du nom de chemin, qui élimine efficacement les séquences ".." et les liens symboliques.

Exécutez votre code à l'aide des droits d'accès les plus réduits possible pour accomplir les tâches nécessaires. Si possible, créez des comptes isolés avec des privilèges limités, qui ne sont utilisés que pour une seule tâche. De cette façon, une attaque réussie ne donnera pas immédiatement accès au reste du logiciel ou de son environnement à l'agresseur. Par exemple, les applications de base de données ne nécessitent que rarement de fonctionner en tant qu'administrateur de base de données, en particulier dans les activités quotidiennes.

Lorsque l'ensemble des objets admissibles, tels que les noms de fichier ou les URLs, est limité ou connu, créez une correspondance à partir d'un ensemble de valeurs d'entrée fixe (tels que des ID numériques) sur les noms de fichiers réels ou sur les URLs, et rejetez toute autre entrée.

Exécutez votre code dans un "bac à sable" ou un environnement similaire, qui impose des limites strictes entre le processus et le système d'exploitation. Cela peut restreindre efficacement quels fichiers sont accessibles dans un répertoire particulier ou lesquels peuvent être exécutés par votre logiciel.

Au niveau du système d'exploitation, on peut citer les exemples pour Unix: chroot, AppArmor et SELinux. En général, le code géré (managed code) peut offrir une certaine protection. Par exemple, java.io.FilePermission dans le SecurityManager de Java permet de spécifier des restrictions sur les opérations de fichiers.

Ceci peut ne pas être une solution adéquate. Par ailleurs, elle limite l'impact au seul système d'exploitation; le reste de votre application peut encore faire l'objet de vulnérabilités.


### Reference


* [ https://owasp.org/www-community/attacks/Path_Traversal ](https://owasp.org/www-community/attacks/Path_Traversal)
* [ https://cwe.mitre.org/data/definitions/22.html ](https://cwe.mitre.org/data/definitions/22.html)


#### CWE Id: [ 22 ](https://cwe.mitre.org/data/definitions/22.html)


#### WASC Id: 33

#### Source ID: 1

### [ Content Security Policy (CSP) Header Not Set ](https://www.zaproxy.org/docs/alerts/10038/)



##### Moyen (Haut)

### Description

Content Security Policy (CSP) is an added layer of security that helps to detect and mitigate certain types of attacks, including Cross Site Scripting (XSS) and data injection attacks. These attacks are used for everything from data theft to site defacement or distribution of malware. CSP provides a set of standard HTTP headers that allow website owners to declare approved sources of content that browsers should be allowed to load on that page — covered types are JavaScript, CSS, HTML frames, fonts, images and embeddable objects such as Java applets, ActiveX, audio and video files.

* URL: http://localhost:8000/register
  * Méthode: `GET`
  * Parameter: ``
  * Attaquer: ``
  * Evidence: ``
  * Other Info: ``

Instances: 1

### Solution

Ensure that your web server, application server, load balancer, etc. is configured to set the Content-Security-Policy header.

### Reference


* [ https://developer.mozilla.org/en-US/docs/Web/Security/CSP/Introducing_Content_Security_Policy ](https://developer.mozilla.org/en-US/docs/Web/Security/CSP/Introducing_Content_Security_Policy)
* [ https://cheatsheetseries.owasp.org/cheatsheets/Content_Security_Policy_Cheat_Sheet.html ](https://cheatsheetseries.owasp.org/cheatsheets/Content_Security_Policy_Cheat_Sheet.html)
* [ https://www.w3.org/TR/CSP/ ](https://www.w3.org/TR/CSP/)
* [ https://w3c.github.io/webappsec-csp/ ](https://w3c.github.io/webappsec-csp/)
* [ https://web.dev/articles/csp ](https://web.dev/articles/csp)
* [ https://caniuse.com/#feat=contentsecuritypolicy ](https://caniuse.com/#feat=contentsecuritypolicy)
* [ https://content-security-policy.com/ ](https://content-security-policy.com/)


#### CWE Id: [ 693 ](https://cwe.mitre.org/data/definitions/693.html)


#### WASC Id: 15

#### Source ID: 3

### [ Missing Anti-clickjacking Header ](https://www.zaproxy.org/docs/alerts/10020/)



##### Moyen (Moyen)

### Description

The response does not include either Content-Security-Policy with 'frame-ancestors' directive or X-Frame-Options to protect against 'ClickJacking' attacks.

* URL: http://localhost:8000/register
  * Méthode: `GET`
  * Parameter: `x-frame-options`
  * Attaquer: ``
  * Evidence: ``
  * Other Info: ``

Instances: 1

### Solution

Modern Web browsers support the Content-Security-Policy and X-Frame-Options HTTP headers. Ensure one of them is set on all web pages returned by your site/app.
If you expect the page to be framed only by pages on your server (e.g. it's part of a FRAMESET) then you'll want to use SAMEORIGIN, otherwise if you never expect the page to be framed, you should use DENY. Alternatively consider implementing Content Security Policy's "frame-ancestors" directive.

### Reference


* [ https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options ](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options)


#### CWE Id: [ 1021 ](https://cwe.mitre.org/data/definitions/1021.html)


#### WASC Id: 15

#### Source ID: 3

### [ Application Error Disclosure ](https://www.zaproxy.org/docs/alerts/90022/)



##### Faible (Moyen)

### Description

This page contains an error/warning message that may disclose sensitive information like the location of the file that produced the unhandled exception. This information can be used to launch further attacks against the web application. The alert could be a false positive if the error message is found inside a documentation page.

* URL: http://localhost:8000/register
  * Méthode: `POST`
  * Parameter: ``
  * Attaquer: ``
  * Evidence: `HTTP/1.1 500 Internal Server Error`
  * Other Info: ``

Instances: 1

### Solution

Review the source code of this page. Implement custom error pages. Consider implementing a mechanism to provide a unique error reference/identifier to the client (browser) while logging the details on the server side and not exposing them to the user.

### Reference



#### CWE Id: [ 200 ](https://cwe.mitre.org/data/definitions/200.html)


#### WASC Id: 13

#### Source ID: 3

### [ X-Content-Type-Options Header Missing ](https://www.zaproxy.org/docs/alerts/10021/)



##### Faible (Moyen)

### Description

The Anti-MIME-Sniffing header X-Content-Type-Options was not set to 'nosniff'. This allows older versions of Internet Explorer and Chrome to perform MIME-sniffing on the response body, potentially causing the response body to be interpreted and displayed as a content type other than the declared content type. Current (early 2014) and legacy versions of Firefox will use the declared content type (if one is set), rather than performing MIME-sniffing.

* URL: http://localhost:8000/register
  * Méthode: `GET`
  * Parameter: `x-content-type-options`
  * Attaquer: ``
  * Evidence: ``
  * Other Info: `This issue still applies to error type pages (401, 403, 500, etc.) as those pages are often still affected by injection issues, in which case there is still concern for browsers sniffing pages away from their actual content type.
At "High" threshold this scan rule will not alert on client or server error responses.`

Instances: 1

### Solution

Ensure that the application/web server sets the Content-Type header appropriately, and that it sets the X-Content-Type-Options header to 'nosniff' for all web pages.
If possible, ensure that the end user uses a standards-compliant and modern web browser that does not perform MIME-sniffing at all, or that can be directed by the web application/web server to not perform MIME-sniffing.

### Reference


* [ https://learn.microsoft.com/en-us/previous-versions/windows/internet-explorer/ie-developer/compatibility/gg622941(v=vs.85) ](https://learn.microsoft.com/en-us/previous-versions/windows/internet-explorer/ie-developer/compatibility/gg622941(v=vs.85))
* [ https://owasp.org/www-community/Security_Headers ](https://owasp.org/www-community/Security_Headers)


#### CWE Id: [ 693 ](https://cwe.mitre.org/data/definitions/693.html)


#### WASC Id: 15

#### Source ID: 3


