FROM openshift/nginx:1.12

EXPOSE 8080
EXPOSE 8443

USER 0

RUN yum install -y centos-release-scl-rh && yum install -y bash curl ca-certificates make java-1.8.0-openjdk-headless findutils inotify-tools rh-ruby23 rh-ruby23-devel rh-ruby23-scldevel rh-ruby23-build baekmuk-ttf-fonts-common  graphviz-devel graphviz libxml2-devel gcc python27-python-pip && yum clean all -y

RUN scl enable rh-ruby23 "gem install --no-document asciidoctor:${ASCIIDOCTOR_VERSION} asciidoctor-confluence:${ASCIIDOCTOR_CONFLUENCE_VERSION} asciidoctor-diagram:${ASCIIDOCTOR_DIAGRAM_VERSION} asciimath asciidoctor-revealjs:${ASCIIDOCTOR_REVEALJS_VERSION} coderay epubcheck:3.0.1 haml kindlegen:3.0.3 pygments.rb rake rouge slim thread_safe tilt graphviz"

RUN scl enable python27 "pip install --upgrade pip" && scl enable python27 "pip install --no-cache-dir actdiag blockdiag[pdf] nwdiag Pygments seqdiag" && rm -rf .cache

RUN echo -n "Directory: " && pwd && ls -l /tmp/src /opt $HOME && echo $HOME

RUN mv /usr/libexec/s2i/assemble /usr/libexec/s2i/assemble-nginx && cp .s2i/bin/assemble /usr/libexec/s2i/assemble

USER 1001
CMD /usr/libexec/s2i/usage
