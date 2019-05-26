FROM openshift/nginx:1.14

EXPOSE 8080
EXPOSE 8443

USER 0

RUN yum install -y centos-release-scl-rh && yum install -y bash curl ca-certificates make java-1.8.0-openjdk-headless findutils inotify-tools rh-ruby23 rh-ruby23-devel rh-ruby23-scldevel rh-ruby23-build baekmuk-ttf-fonts-common  graphviz-devel graphviz libxml2-devel gcc python27-python-pip && yum clean all -y

RUN scl enable rh-ruby23 "gem install --no-document nokogiriasciidoctor:${ASCIIDOCTOR_VERSION} asciidoctor-pdf:${ASCIIDOCTOR_PDF_VERSION} asciidoctor-confluence:${ASCIIDOCTOR_CONFLUENCE_VERSION} asciidoctor-diagram:${ASCIIDOCTOR_DIAGRAM_VERSION} mathematical ruby-enum asciidoctor-mathematical:${ASCIIDOCTOR_MATHEMATICAL_VERSION} asciimath asciidoctor-revealjs:${ASCIIDOCTOR_REVEALJS_VERSION} asciidoctor-bibliography:${ASCIIDOCTOR_BIBLIOGRAPHY_VERSION} coderay epubcheck:3.0.1 haml kindlegen:3.0.3 pygments.rb rake rouge slim thread_safe tilt graphviz"

RUN scl enable python27 "pip install --upgrade pip" && scl enable python27 "pip install --no-cache-dir actdiag blockdiag[pdf] nwdiag Pygments seqdiag" && rm -rf .cache

RUN sed -i -e 's/set -e/set -e\n\
\n\
cp -a \/tmp\/src\/* .\n\
\n\
if [ !  -f index.adoc ] ; then\n\
    echo "===================================================="\n\
    echo "   No index.adoc!!!!"\n\
    echo "===================================================="\n\
    exit 1\n\
fi\n\
\n\
for adoc in `find . -iname "*.adoc"` ; do\n\
    echo "---> Converting: ${adoc} ..."\n\
    scl enable python27 rh-ruby23 "asciidoctor -b xhtml5 --safe -n -v -t -r asciidoctor-diagram \"${adoc}\""\n\
    scl enable python27 rh-ruby23 "asciidoctor-pdf -b xhtml5 --safe -n -v -t -r asciidoctor-diagram \"${adoc}\""\n\
    rm "${adoc}"\n\
done\n\
/' ${STI_SCRIPTS_PATH}/assemble

USER 1001
CMD ${STI_SCRIPTS_PATH}/usage
