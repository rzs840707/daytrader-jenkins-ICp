FROM websphere-liberty:kernel
COPY wlp/usr/servers/Daytrader3Sample /opt/ibm/wlp/usr/servers/defaultServer
COPY repositories.properties /opt/ibm/wlp/etc/repositories.properties
RUN installUtility install --acceptLicense defaultServer
