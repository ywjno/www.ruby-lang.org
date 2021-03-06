---
layout: news_post
title: "Vulnérabilité dans le module Net::HTTPS"
author: "Jean-Denis Vauguet"
lang: fr
---

Un trou de sécurité a été détecté dans la bibliothèque net/https.
[Informations détaillées par le rapporteur][1] ou ci-dessous (en
anglais).

#### Impact

The vulnerability exists in the connect method within http.rb file which
fails to call post\_connection\_check after the SSL connection has been
negotiated. Since the server certificate\'s CN is not validated against
the requested DNS name, the attacker can impersonate the target server
in a SSL connection. The integrity and confidentiality benefits of SSL
are thereby eliminated.

#### Vulnerable versions

1.8 series
: * 1\.8.4 and all prior versions
  * 1\.8.5-p113 and all prior versions
  * 1\.8.6-p110 and all prior versions

Development version (1.9 series)
: All versions before 2006-09-23

#### Solution

1.8 series

: Please upgrade to 1.8.6-p111 or 1.8.5-p114.

  * [&lt;URL:http://ftp.ruby-lang.org/pub/ruby/1.8/ruby-1.8.6-p111.tar.gz&gt;][2]
  * [&lt;URL:http://ftp.ruby-lang.org/pub/ruby/1.8/ruby-1.8.5-p114.tar.gz&gt;][3]

  Then you should use Net::HTTP#enable\_post\_connection\_check= to
  enable post\_connection\_check.

      http = Net::HTTP.new(host, 443)
      http.use_ssl = true
      http.enable_post_connection_check = true
      http.verify_mode = OpenSSL::SSL::VERIFY_PEER
      store = OpenSSL::X509::Store.new
      store.set_default_paths
      http.cert_store = store
      http.start {
        response = http.get("/")
      }

  Please note that a package that corrects this weakness may already be
  available through your package management software.

Development version (1.9 series)
: Please update your Ruby to a version after 2006-09-23. The default
  value of Net::HTTP#enable\_post\_connection\_check is true on Ruby
  1.9.

#### Changes

* 2007-10-04 16:30 +09:00 added description for
  enable\_post\_connection\_check to \`Solution\'.



[1]: http://www.isecpartners.com/advisories/2007-006-rubyssl.txt
[2]: http://ftp.ruby-lang.org/pub/ruby/1.8/ruby-1.8.6-p111.tar.gz
[3]: http://ftp.ruby-lang.org/pub/ruby/1.8/ruby-1.8.5-p114.tar.gz
