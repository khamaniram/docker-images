ARG BASE_IMAGE=alpine

FROM $BASE_IMAGE

RUN \
  # Install openldap.
  apk add --update openldap openldap-back-hdb openldap-clients \
  \
  # Clean sources list.
  && rm -rf /var/cache/apk/*

# Use default config for LDAP database
RUN mv /var/lib/openldap/openldap-data/DB_CONFIG.example /var/lib/openldap/openldap-data/DB_CONFIG

COPY ./files /

HEALTHCHECK --interval=10s --retries=5 --timeout=5s \
  CMD ldapwhoami  -x -H ldap://127.0.0.1:3890/ -D "cn=Manager,dc=glpi,dc=org" -w insecure || exit 1

CMD ["slapd", "-d", "32768", "-h", "ldap://0.0.0.0:3890/"]
 