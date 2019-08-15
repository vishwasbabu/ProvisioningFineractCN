# Provisioning Fineract-CN
Instructions and sample scripts for manually provisioning the [Apache Fineract-CN suite of projects](https://github.com/search?utf8=%E2%9C%93&q=apache/fineract-cn)

Additional Information on Fineract-CN is available at https://cwiki.apache.org/confluence/display/FINERACT/Fineract+CN


## Running the project locally

This section describes how you can build and run the microservices on your local box.

### Requirements

The following tools/services are required for bringing up Fineract-CN

* Java 1.8.x
* nodeJs v6.10+ and npm 3+
* Postman
* Cassandra 3.11+
* MySQL 5.7+
* ActiveMQ 5+

Ensure Cassandra, MySQL and ActiveMQ are running before proceeding further

### Generating the RSA keypair

Generate the RSA keypair details by following the instructions at https://github.com/apache/fineract-cn-lang#generate-and-print-rsa-keys. At the end of this process, you should have information similar to the following available

````
--------- Public Key------------------
--system.publicKey.exponent=65537
--system.publicKey.timestamp=2019-07-05T08_19_55
--system.publicKey.modulus=20464695676860611938856209625229220215026883327670723302048622443806856058778908096610821995940450235744170336036362329301282210600550800159151077278576885128944844759127874515764241356524632595213982779981877023485472619778711017353164889942073020965948922195940952800512202493532579358814388867792909327011639605961931267930289232684683998117883372801739296790347414949211395715294928098885866495006348709373823237305845297129169321692920303404853680091437179514780162396587111176924982831007694276976401511297345417648123931946457210214721785741609080708480633684168246817154747003491681474693497600023219227806079 

--------- Private Key------------------

--system.privateKey.modulus=20464695676860611938856209625229220215026883327670723302048622443806856058778908096610821995940450235744170336036362329301282210600550800159151077278576885128944844759127874515764241356524632595213982779981877023485472619778711017353164889942073020965948922195940952800512202493532579358814388867792909327011639605961931267930289232684683998117883372801739296790347414949211395715294928098885866495006348709373823237305845297129169321692920303404853680091437179514780162396587111176924982831007694276976401511297345417648123931946457210214721785741609080708480633684168246817154747003491681474693497600023219227806079 
--system.privateKey.exponent=4119044214313568397782203353317952360596603719659192686227983866461025653773480273775017973792674047632655612900432663774863262584980478277909606182487872373406037005319984020285428196803583144841351402272623705949263306643590292962839282591908162100215637467181242784862847904118105105850444841159288141547625922803397739072527571820037767402092994225405003543601678099654500699957318148724490236267361778707785162967441827702526251374810367408637903375979839536079100058321690207424519435561620108275933474187991634919310051372237561264452169739483057032022247604226156076536907349003664464115274065541313927326513   

````


### Deploying the microservices

Each microservice your wish to provision needs to be built, published to maven local (optional) and deployed. Instructions for deploying the core functional set of microservices follow, the procedure for deploying other microservices would be similar

#### Provisioner
The sequence of commands for deploying the provisioner microservice on a *nix based system are illustrated below.
Ensure your runtime arguments, i.e RSA key properties and other external service connection properties are updated as appropriate

```
git clone https://github.com/apache/fineract-cn-provisioner

cd fineract-cn-provisioner/

sh gradlew publishToMavenLocal


java -jar service/build/libs/service-0.1.0-BUILD-SNAPSHOT-boot.jar -Drun.arguments= --cassandra.contactPoints=127.0.0.1:9042 --cassandra.clusterName=Datacenter1 --cassandra.cluster.user=cassandra --cassandra.cluster.pwd=password --mariadb.host=localhost --mariadb.user=root --mariadb.password=mysql --server.port=2020 --system.publicKey.exponent=65537 --system.publicKey.timestamp=2019-07-05T08_19_55 --system.publicKey.modulus=20464695676860611938856209625229220215026883327670723302048622443806856058778908096610821995940450235744170336036362329301282210600550800159151077278576885128944844759127874515764241356524632595213982779981877023485472619778711017353164889942073020965948922195940952800512202493532579358814388867792909327011639605961931267930289232684683998117883372801739296790347414949211395715294928098885866495006348709373823237305845297129169321692920303404853680091437179514780162396587111176924982831007694276976401511297345417648123931946457210214721785741609080708480633684168246817154747003491681474693497600023219227806079 --system.privateKey.modulus=20464695676860611938856209625229220215026883327670723302048622443806856058778908096610821995940450235744170336036362329301282210600550800159151077278576885128944844759127874515764241356524632595213982779981877023485472619778711017353164889942073020965948922195940952800512202493532579358814388867792909327011639605961931267930289232684683998117883372801739296790347414949211395715294928098885866495006348709373823237305845297129169321692920303404853680091437179514780162396587111176924982831007694276976401511297345417648123931946457210214721785741609080708480633684168246817154747003491681474693497600023219227806079 --system.privateKey.exponent=4119044214313568397782203353317952360596603719659192686227983866461025653773480273775017973792674047632655612900432663774863262584980478277909606182487872373406037005319984020285428196803583144841351402272623705949263306643590292962839282591908162100215637467181242784862847904118105105850444841159288141547625922803397739072527571820037767402092994225405003543601678099654500699957318148724490236267361778707785162967441827702526251374810367408637903375979839536079100058321690207424519435561620108275933474187991634919310051372237561264452169739483057032022247604226156076536907349003664464115274065541313927326513 --system.initialclientid=service-runner


```

#### Identity
The sequence of commands for deploying the Identity microservice on a *nix based system are illustrated below.

```
git clone https://github.com/apache/fineract-cn-identity

cd fineract-cn-identity/

sh gradlew publishToMavenLocal

java -jar service/build/libs/service-0.1.0-BUILD-SNAPSHOT-boot.jar -Drun.arguments= --cassandra.contactPoints=127.0.0.1:9042 --cassandra.clustername=Datacenter1 --cassandra.cluster.user=cassandra --cassandra.cluster.pwd=password --mariadb.host=localhost --mariadb.user=root --mariadb.password=mysql --server.port=2021 --system.publicKey.exponent=65537 --system.publicKey.timestamp=2019-07-05T08_19_55 --system.publicKey.modulus=20464695676860611938856209625229220215026883327670723302048622443806856058778908096610821995940450235744170336036362329301282210600550800159151077278576885128944844759127874515764241356524632595213982779981877023485472619778711017353164889942073020965948922195940952800512202493532579358814388867792909327011639605961931267930289232684683998117883372801739296790347414949211395715294928098885866495006348709373823237305845297129169321692920303404853680091437179514780162396587111176924982831007694276976401511297345417648123931946457210214721785741609080708480633684168246817154747003491681474693497600023219227806079

```

#### Office
The sequence of commands for deploying the Identity microservice on a *nix based system are illustrated below.

```
git clone https://github.com/apache/fineract-cn-office

cd fineract-cn-office/

sh gradlew publishToMavenLocal

java -jar service/build/libs/service-0.1.0-BUILD-SNAPSHOT-boot.jar -Drun.arguments= --cassandra.contactPoints=127.0.0.1:9042 --cassandra.clustername=Datacenter1 --cassandra.cluster.user=cassandra --cassandra.cluster.pwd=password --mariadb.host=localhost --mariadb.user=root --mariadb.password=mysql --server.port=2023 --system.publicKey.exponent=65537 --system.publicKey.timestamp=2019-07-05T08_19_55 --system.publicKey.modulus=20464695676860611938856209625229220215026883327670723302048622443806856058778908096610821995940450235744170336036362329301282210600550800159151077278576885128944844759127874515764241356524632595213982779981877023485472619778711017353164889942073020965948922195940952800512202493532579358814388867792909327011639605961931267930289232684683998117883372801739296790347414949211395715294928098885866495006348709373823237305845297129169321692920303404853680091437179514780162396587111176924982831007694276976401511297345417648123931946457210214721785741609080708480633684168246817154747003491681474693497600023219227806079

```

#### Customer management
The sequence of commands for deploying the Customer microservice on a *nix based system are illustrated below.

```
git clone https://github.com/apache/fineract-cn-customer

cd fineract-cn-customer/

sh gradlew publishToMavenLocal

java -jar service/build/libs/service-0.1.0-BUILD-SNAPSHOT-boot.jar -Drun.arguments= --cassandra.contactPoints=127.0.0.1:9042 --cassandra.clustername=Datacenter1 --cassandra.cluster.user=cassandra --cassandra.cluster.pwd=password --mariadb.host=localhost --mariadb.user=root --mariadb.password=mysql --server.port=2024 --system.publicKey.exponent=65537 --system.publicKey.timestamp=2019-07-05T08_19_55 --system.publicKey.modulus=20464695676860611938856209625229220215026883327670723302048622443806856058778908096610821995940450235744170336036362329301282210600550800159151077278576885128944844759127874515764241356524632595213982779981877023485472619778711017353164889942073020965948922195940952800512202493532579358814388867792909327011639605961931267930289232684683998117883372801739296790347414949211395715294928098885866495006348709373823237305845297129169321692920303404853680091437179514780162396587111176924982831007694276976401511297345417648123931946457210214721785741609080708480633684168246817154747003491681474693497600023219227806079

```

#### Deposit management
The sequence of commands for deploying the Deposist microservice on a *nix based system are illustrated below.

```
git clone https://github.com/apache/fineract-cn-deposit-account-management

cd fineract-cn-deposit-account-management/

sh gradlew publishToMavenLocal

java -jar service/build/libs/service-0.1.0-BUILD-SNAPSHOT-boot.jar -Drun.arguments= --cassandra.contactPoints=127.0.0.1:9042 --cassandra.clustername=Datacenter1 --cassandra.cluster.user=cassandra --cassandra.cluster.pwd=password --mariadb.host=localhost --mariadb.user=root --mariadb.password=mysql --server.port=2026 --system.publicKey.exponent=65537 --system.publicKey.timestamp=2019-07-05T08_19_55 --system.publicKey.modulus=20464695676860611938856209625229220215026883327670723302048622443806856058778908096610821995940450235744170336036362329301282210600550800159151077278576885128944844759127874515764241356524632595213982779981877023485472619778711017353164889942073020965948922195940952800512202493532579358814388867792909327011639605961931267930289232684683998117883372801739296790347414949211395715294928098885866495006348709373823237305845297129169321692920303404853680091437179514780162396587111176924982831007694276976401511297345417648123931946457210214721785741609080708480633684168246817154747003491681474693497600023219227806079

```

#### Accounting
The sequence of commands for deploying the Accounting microservice on a *nix based system are illustrated below.

```
git clone https://github.com/apache/fineract-cn-accounting

cd fineract-cn-accounting/

sh gradlew publishToMavenLocal

java -jar service/build/libs/service-0.1.0-BUILD-SNAPSHOT-boot.jar -Drun.arguments= --cassandra.contactPoints=127.0.0.1:9042 --cassandra.clustername=Datacenter1 --cassandra.cluster.user=cassandra --cassandra.cluster.pwd=password --mariadb.host=localhost --mariadb.user=root --mariadb.password=mysql --server.port=2025 --system.publicKey.exponent=65537 --system.publicKey.timestamp=2019-07-05T08_19_55 --system.publicKey.modulus=20464695676860611938856209625229220215026883327670723302048622443806856058778908096610821995940450235744170336036362329301282210600550800159151077278576885128944844759127874515764241356524632595213982779981877023485472619778711017353164889942073020965948922195940952800512202493532579358814388867792909327011639605961931267930289232684683998117883372801739296790347414949211395715294928098885866495006348709373823237305845297129169321692920303404853680091437179514780162396587111176924982831007694276976401511297345417648123931946457210214721785741609080708480633684168246817154747003491681474693497600023219227806079

```


### Initializing the application

Once all services are started we will have to create an initial tenant, provision services for the same and then create a login user for the tenant.

#### Provisioning the Microservices

We provide a postman-request-collection as well as a postman-environment that defines variables that are used to hold values received in responses. Both files are located under 
[postman-initial-requests folder](https://github.com/vishwasbabu/ProvisioningFineractCN):
```
ProvisioningFineractCN/Fineract-Initial-Setup-Environment.postman_environment.json
ProvisioningFineractCN/Fineract-Initial-Requests.postman_collection.json
```

We would initialize postman as follows:

1. Start Postman and load both files into Postman by clicking ```Import``` and then selecting the file.
2. You will see the collection "Fineract-Initial-Requests" in the left sidebar.
3. Open the collection by clicking on it.
4. Select the environment "Fineract-Initial-Setup-Environment" in the environment drop-down (top right corner in Postman).
5. Execute the requests one by one by selecting them in the collection and then pressing "Send".


The first request will retrieve a token. The response should look like this, with a different token:

```
{
    "token": "Bearer eyJhbGciOiJSUzUxMiJ9.eyJhdWQiOiJwcm92aXNpb25lci12MSIsInN1YiI6IndlcGVtbmVmcmV0IiwiL21pZm9zLmlvL3NpZ25hdHVyZVRpbWVzdGFtcCI6IjIwMTctMDQtMThUMDlfNDRfMjIiLCIvbWlmb3MuaW8vdG9rZW5Db250ZW50IjoiUk9MRV9BRE1JTiIsImlzcyI6InN5c3RlbSIsImlhdCI6MTUwMDA1NjgxNywiZXhwIjoxNTAwNDE2ODE3fQ.OfxTUTStJbKQc4rAPW5PLIQYNjCG_uqcNPR4up6pIQBWLDxkgEiU9EF1WrB5NQdzXBJIHqjDFQpaVywm5DersIh4LxPGD3MZj3TqZK5_LUcZvBDTa4Xgb41e3xXkWB4TkN6KqfmiK12Ngjrrj7qZGBdtypDmFmZwKQRZIOL6T3QbI7LpbPGpeWjpWZirFgtcn5B1Z_h3r9rirCzecUdVjlaplQufxDuVFJS0R3N67pyuGQENvCAC716ID5KbokTQtITXfjnCztFuQBbtCPcYLIzxsKv_-E5k6Gd0pv01OC0XpY3NSgfAolVVgvSXKoRnL3NwAMP2yuzX6i8hR_q82Q",
    "accessTokenExpiration": "2019-07-18T22:26:57.784"
}
```

If you don't get a token there is something wrong with your setup. The token is necessary for authentication in other requests thus be sure that this steps works.

Important: Be sure to execute the requests in the right order! If you execute the requests that gives you the initial password (request "03.2 Create Identity Service for Tenant") twice you will not be able to retrieve the initial password again (due to the implementation of the identity service).
If that happens the variable antonyUserPassword is empty (undefined) and you will not be able to sign in antony and change his password (03.3, 03.4).


#### Debugging help

1. Check if the microservices to which the requests are made are up and running. Check the terminal for details of failures (if any).
2. Reach out to https://lists.apache.org/list.html?dev@fineract.apache.org with the relevant details

#### Resetting the databases if something goes wrong
 
The easiest way to reset the databases is to drop the keyspace/database from ```cassandra``` and ```mariadb``` respectively. 

You can use the following commands to do the same

On Cassandra
```
drop keyspace playground;
drop keyspace seshat;
```  

On MySQL
```
drop database playground;
drop database seshat;
```

### Sign-in using fims-web-app

Prerequisites: Fineract-CN has been successfully provisioned by following the instructions in the previous sections
User ```mifos``` is created in the the last two requests (user creation and role assignment) in the postman request-list. This user has admin rights and is able to manage offices,customers and thier deposits.

Setup https://github.com/apache/fineract-cn-fims-web-app , Navigate to http://localhost:4200 in your browser and enter the credentials of the user you want to sign in with.

The following user-profile is available in fims-web-app after above setup was completed successfully:

```
tenant: playground
user: mifos
password: password
```



## Extending the project

### Remote debugging

****In Progress********

### Project Setup in IntelliJ IDEA

****In Progress********

## References

Derived from https://github.com/apache/fineract-cn-demo-server and https://github.com/senacor/BankingInTheCloud-Fineract

