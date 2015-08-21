# UsingSolr5.2


1. Install Solr
  * run install service command
  * start service
  * access SOLR URL
2. Add libraries
  * MySQL Connector: http://dev.mysql.com/downloads/connector/j/ -> /opt/solr/contrib/dataimporthandler/lib
3. Setup a new collection
  * create folders
    * /opt/solr/example/teste/conf
    * /opt/solr/example/teste/data
  * copy sample from /opt/solr/server/solr/configsets/data_driven_schema_configs/
  * chown copied folder
  * edit solrconfig.xml
4. Create configuration files
  * add data-import.xml
```xml
<dataConfig>
<dataSource type="JdbcDataSource"
            driver="com.mysql.jdbc.Driver"
            url="jdbc:mysql://localhost:3306/core"
            user="root"
            password=""/>
<document>
  <entity name="messages"
    pk="id"
    query="select id,message from messages"
    deltaImportQuery="SELECT id,message from messages WHERE id='${dih.delta.id}'"
    deltaQuery="SELECT id FROM messages WHERE created > '${dih.last_index_time}'"
    >
     <field column="id" name="id"/>
     <field column="message" name="message"/>
  </entity>
</document>
</dataConfig>
```
5. Create Core from UI
  * instanceDir: /opt/solr/example/teste/
  * dataDir: /opt/solr/example/teste/data/ 
6. Reload service + create Core again to trigger presence/detection

# Test
1. Import via DIH UI
  * import as it is
  * query as it is 
2. Change indexed fields
  * add field in schema.xml, reload & reindex
  * query for changes
