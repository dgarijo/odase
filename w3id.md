## Doing content negotiation with w3id

[w3id.org](https://github.com/perma-id/w3id.org/) is a permanent URL service that mints permanent identifiers for online resources. Having a permanent identifier is helpful because URLs tend to change often (e.g., an ontology published on a temporary URL, moved later to another server).
W3ids are `proxy` URLs that do not change. Therefore, if you use a w3id, your resource may be available even if you change the server where it is hosted.

**Benefits**: 
* You do not need to take care of URLs being deprecated (in a report, paper, etc.), as long as you redirect them to a working server.
* You havce full control on the redirection.

**Drawbacks**:
* You do not have control over the w3id itself (i.e., the server where it's hosted). If the service is taken down, your w3id will no longer work. However there is a community of 7 companies (plus partial support from the W3C) to ensure the service is running.


### Example
Let's assume I create the ontology `example` using the w3id `https://w3id.org/example#`. If I publish it in my server (e.g. GitHub), the ontology will live at: `https://dgarijo.github.io/example/release/1.0.1/index-en.html` for the documentation and `https://dgarijo.github.io/example/release/1.0.1/ontology.ttl` for the TTL serialization.

By using `https://w3id.org/example`, I can enable content negotiation and redirect to the resource based on what I receive in the server. For example, if I create a new `example` identifier in `https://github.com/perma-id/w3id.org/blob/master/example/`, and I create an `.htaccess` file I can do the following redirection for HTML:

```
RewriteCond %{HTTP_ACCEPT} !application/rdf\+xml.*(text/html|application/xhtml\+xml)
RewriteCond %{HTTP_ACCEPT} text/html [OR]
RewriteCond %{HTTP_ACCEPT} application/xhtml\+xml [OR]
RewriteCond %{HTTP_USER_AGENT} ^Mozilla/.*
RewriteRule ^$ https://dgarijo.github.io/example/release/1.0.1/index-en.html [R=303,L]
```

and for the TTL file:
```
RewriteCond %{HTTP_ACCEPT} text/turtle [OR]
RewriteCond %{HTTP_ACCEPT} text/\* [OR]
RewriteCond %{HTTP_ACCEPT} \*/turtle
RewriteRule ^$ https://dgarijo.github.io/example/release/1.0.1/ontology.ttl [R=303,L]
```
Now, when I do a request to `https://w3id.org/example`, I will get the HTML representation if I am on a browser, or the TTL representation if I indicate it in my request. For example, the following request acomplishes this:

```bash
curl -sH "Accept:text/turtle"  https://w3id.org/okn/o/sd -L
```

Basically, using the same ontology URI (`https://w3id.org/example`) I can open it in my browser or in Protégé without changing anything else.