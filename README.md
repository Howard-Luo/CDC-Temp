# CDC-Temp


# Start all ###
### start services via systemctl ###
```
systemctl start kafka-zookeeper
systemctl start kafka-server
systemctl start schema-registry
systemctl start kafka-ui
systemctl start db2
systemctl start informix
```
### Start Kafka-UI manually
```
set -xv
podman run --rm --name kafka-ui \
-p 8080:8080 \
-e KAFKA_CLUSTERS_0_NAME=cdcdemo \
-e KAFKA_CLUSTERS_0_BOOTSTRAPSERVERS=cdc01.de.ibm.com:9092 \
-e KAFKA_CLUSTERS_0_SCHEMAREGISTRY=http://cdc01.de.ibm.com:8081 \
-d provectuslabs/kafka-ui:latest
```
### Access Kafka UI
```
http://cdc01.de.ibm.com:8080/ui/clusters/cdcdemo/all-topics?perPage=25
```

### Start Db2 and Informix manually
```
su db2inst1 -c '. $HOME/.bash_profile; db2start;'
su informix -c '. $HOME/.bash_profile; oninit;'
```
### Start CDC Engines
```
su cdcaccess -c '/opt/cdcaccess/bin/dmaccessserver &'
su cdcorax -c '/opt/cdcxstream/bin/dmts64 -I linux &'
su cdcora -c '/opt/cdcoracle/bin/dmts64 -I linux &'
su cdcflat -c '/opt/cdcflat/bin/dmts64 -I linux &'
su cdckafka -c '/opt/cdckafka/bin/dmts64 -I linux &'
su db2inst1 -c '/opt/cdcdb2/bin/dmts64 -I linux &'
su informix -c '/opt/cdcifx/bin/dmts64 -I linux &'
su postgres -c '/opt/cdcpsql/bin/dmts64 -I linux &'
```


# Stop all
### Stop CDC Engines
```
su cdcorax -c '/opt/cdcxstream/bin/dmshutdown -I linux &'
su cdcora -c '/opt/cdcoracle/bin/dmshutdown -I linux &'
su cdcflat -c '/opt/cdcflat/bin/dmshutdown -I linux &'
su cdckafka -c '/opt/cdckafka/bin/dmshutdown -I linux &'
su db2inst1 -c '/opt/cdcdb2/bin/dmshutdown -I linux &'
su informix -c '/opt/cdcifx/bin/dmshutdown -I linux &'
su postgres -c '/opt/cdcpsql/bin/dmshutdown -I linux &'
su cdcaccess -c '/opt/cdcaccess/bin/dmshutdownserver &'
```
### stop services ###
```
systemctl start db2
systemctl start informix
systemctl start kafka-ui
systemctl start schema-registry
systemctl start kafka-server
systemctl start kafka-zookeeper
```
### Stop Db2 and Informix manually
```
su db2inst1 -c '. $HOME/.bash_profile; db2stop force;'
su informix -c '. $HOME/.bash_profile; onmode -ucky;'
```
### Stop Kafka-UI
```
podman stop --all
```
