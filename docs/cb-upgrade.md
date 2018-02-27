## Upgrade Cloudbreak

To upgrade Cloudbreak to the newest version, perform the following steps.

We recommend that you back up Cloudbreak databases before upgrading. Refer to [Back up Cloudbreak Database](cb-migrate.md#back-up-cloudbreak-database).

**Steps**


1. On the VM where Cloudbreak is running, navigate to the directory where your Profile file is located:

    <pre>cd /var/lib/cloudbreak-deployment/</pre>

2. Stop all of the running Cloudbreak components:

    <pre>cbd kill</pre>
    
3. Update Cloudbreak deployer:

    <pre>cbd update</pre>
    
3. Update the `docker-compose.yml` file with new Docker containers needed for the cbd:

    <pre>cbd regenerate</pre>
    
4. If there are no other Cloudbreak instances that still use old Cloudbreak versions, remove the obsolete containers:

    <pre>cbd util cleanup</pre>
    
5. Check the health and version of the updated cbd:

    <pre>cbd doctor</pre>
    
6. Start the new version of the cbd:

    <pre>cbd start</pre>
    
    Cloudbreak needs to download updated docker images for the new version, so this step may take a while.

    
Example:

<pre>
[cloudbreak@ip-172-31-15-208 ~]$ cd /var/lib/cloudbreak-deployment/
[cloudbreak@ip-172-31-15-208 cloudbreak-deployment]$ cbd kill
Stopping cbreak_traefik_1 ... done
Stopping cbreak_logspout_1 ... done
Stopping cbreak_cloudbreak_1 ... done
Stopping cbreak_registrator_1 ... done
Stopping cbreak_logsink_1 ... done
Stopping cbreak_consul_1 ... done
Stopping cbreak_commondb_1 ... done
Stopping cbreak_sultans_1 ... done
Stopping cbreak_periscope_1 ... done
Stopping cbreak_mail_1 ... done
Stopping cbreak_uluwatu_1 ... done
Stopping cbreak_identity_1 ... done
Stopping cbreak_haveged_1 ... done
Going to remove cbreak_traefik_1, cbreak_logspout_1, cbreak_cloudbreak_1, cbreak_registrator_1, cbreak_logsink_1, cbreak_consul_1, cbreak_commondb_1, cbreak_sultans_1, cbreak_periscope_1, cbreak_mail_1, cbreak_uluwatu_1, cbreak_smartsense_1, cbreak_identity_1, cbreak_haveged_1
Removing cbreak_traefik_1 ... done
Removing cbreak_logspout_1 ... done
Removing cbreak_cloudbreak_1 ... done
Removing cbreak_registrator_1 ... done
Removing cbreak_logsink_1 ... done
Removing cbreak_consul_1 ... done
Removing cbreak_commondb_1 ... done
Removing cbreak_sultans_1 ... done
Removing cbreak_periscope_1 ... done
Removing cbreak_mail_1 ... done
Removing cbreak_uluwatu_1 ... done
Removing cbreak_smartsense_1 ... done
Removing cbreak_identity_1 ... done
Removing cbreak_haveged_1 ... done
[cloudbreak@ip-172-31-15-208 cloudbreak-deployment]$ cbd update
Updating /usr/bin/cbd from url: https://github.com/hortonworks/cloudbreak-deployer/releases/download/v2.4.0/cloudbreak-deployer_2.4.0_Linux_x86_64.tgz
mv: try to overwrite '/usr/bin/cbd', overriding mode 0755 (rwxr-xr-x)? y
[cloudbreak@ip-172-31-15-208 cloudbreak-deployment]$ cbd regenerate
Generating Cloudbreak client certificate and private key in /var/lib/cloudbreak-deployment/certs with ec2-54-200-79-120.us-west-2.compute.amazonaws.com into /var/lib/cloudbreak-deployment/certs/traefik.
renaming: docker-compose.yml to: docker-compose-20180227-170609.yml
generating docker-compose.yml
renaming: uaa.yml to: uaa-20180227-170609.yml
generating uaa.yml
[cloudbreak@ip-172-31-15-208 cloudbreak-deployment]$ cbd util cleanup
Found old/different versioned images based on docker-compose.yml file: traefik:v1.2.0 hortonworks/cbd-smartsense:0.10.0 hortonworks/cloudbreak-uaa:3.6.5 hortonworks/cloudbreak:1.16.5 hortonworks/cloudbreak-autoscale:1.16.5
Untagged: traefik:v1.2.0
Deleted: sha256:76ac035b61730505f7abcc7da5f77a595690ba01e11a17d84b08b20f8a2576d2
Deleted: sha256:f57a10926975056ca6942c2179661fd4ff9b83818fb0ad95e8cdaa85a605932f
Deleted: sha256:f0df05825751b7a76d9e231c19ae0e323301e76ad20380262a74b364fe58f508
Untagged: hortonworks/cbd-smartsense:0.10.0
Deleted: sha256:f5693f049ae1c9ec6d9cf184b5c1ab91471a9e6f7d60b93eacfdf8c5ccc028b1
Deleted: sha256:fa76f51111b034f67830f0d3a4e6a7932e58a484ee7591ccefb37344cee1190a
Deleted: sha256:eedfe4aa2047520fd9f93f60a87db15c9a059b775b575d08dd85c4fd54d43928
Deleted: sha256:480ade3ce391521428a116f8ce55f73127fd58552affdc1db8cd7d62a8e10e63
Deleted: sha256:137d9905d1d14d5f643db38873ed1b9309edcbd498c5d7370263157bacbef58f
Deleted: sha256:11e94a33cff8b9708bc6a5cc53181d0331ba750711ac997a05b122cdf90023ea
Deleted: sha256:8bb6faa3c31cb954647285c9a02a67f74f992b33f095ded7cbfb1e4f2a6c36a9
Deleted: sha256:64909872470f59920a751005b05aa1441dd0226c7bc5112d7866adf33339be79
Deleted: sha256:ec7ac4c4c628df79fd349cb41aeb2809e190dee86d9bf3b1a349a7f6962cc173
Deleted: sha256:fd6befce73f03cd5c3c4a815d22601227a462a3a7a3e93962ffc4eb2e8687c14
Deleted: sha256:5082dcbba71d261b3ac4ca8f2566e963ed147a882aea51388adf682ab8d8464b
Deleted: sha256:569db369601fec561a8af0ca5844f86e301729074be023c4318c22227d20d4fb
Deleted: sha256:3a58fd412c420874ca49ede0886e7fcdd9626643578ddcc800df122671361947
Deleted: sha256:dc1e2dcdc7b6ff86d785fa16cf97464d263d04346a191c57b5ca8a66b4155861
Untagged: hortonworks/cloudbreak-uaa:3.6.5
Deleted: sha256:db63f7f84ac578b45ce26c38893523bba781046f1516e38853edb358edaafb23
Deleted: sha256:61fc43a62226488ba4bfcdb6f61f4a4ca1092f6e8ef4add196092f8fe7a2476d
Deleted: sha256:beda3bf160551e19c4602cf888ee142cdf4f01f6dcc8f63535d5825c82125f7c
Deleted: sha256:c8057ee0aedf424f9682d0bdea5433eeaa72c0c2ceced56cdb5d59ff57a69bc9
Deleted: sha256:a731376fcabf68ce8e5fbf62f35b6e42af71a2ccca0f30277133d55dc06b2278
Deleted: sha256:9ff792b2eecf9ea73d2624ef2579cb6d061b84ab6955b102afd52c83590c2c90
Deleted: sha256:ad080c523a11aed248eb92f4a015db4fb1c7368a098dc921ee38deae430764db
Deleted: sha256:655a0c9f12e720e6b0b8165db074aefdba6a596d7ebb78f8466f01049bc5dbd3
Deleted: sha256:cee64c24e2ba03e634c608d44db4857a4b2c4b7826a34134d0cad160c89a598c
Deleted: sha256:85c5e3425c25b30e59ef7de7a186ddada5edb6af03f5c944f658698faf4e7532
Deleted: sha256:2dde4252d1b9928be576eb1c37782d1988b39b5344c3c5b9af034d68836f90ab
Deleted: sha256:5be0fb17f150419697de60eafa7702f79928b3c2bc3b79c8497d6e434b93f311
Deleted: sha256:c6b22d286b1ea69ea30ee5cd05ec3aa3a31cc8a11ce3c3a72c33cb888a97834d
Deleted: sha256:aa861d5f842e36c68b33686baa505e6d0a7c4fa3d255ae2e382ef53dd79b3b66
Deleted: sha256:00101c7a2746aa757935d6583266c7f9db737e25a3411426f5dc03be78c2f8db
Deleted: sha256:b60d29b508b1cd73e1c4f812f910ff8457f6839342bdb49a932ae1819801dd85
Deleted: sha256:0f34203a1ec5e79b2a2b7ba54a0c35e0c64ac2e056866a3127d6b8b0b58a972e
Deleted: sha256:dba81ca9cb6711840fe6b7767ea1b4fb069efdfdd0fba1df07dc499d69fa6313
Deleted: sha256:8983739a3785ca7c1472278d134bc86c718334e859b85cb84ca3837792ffd856
Deleted: sha256:5f4d84cdff0a1cffda8113a71cb77d34743885851bd2678f3e7ac211c6602995
Deleted: sha256:070ea54dbf072b40a164d6986d055ced72b7244cd24b9e36cc26f543825328ea
Deleted: sha256:ba1503e7997a82c75c84fe886002f3c2dddf9fa596eb0d3535ed6dc2494d3f3b
Deleted: sha256:e62142644f2d2a1a2699e6635b4342d7f827bea256cdee58a5c6701b75dfd580
Deleted: sha256:0c650b3801622851e7b86058f914be69af2d1425ab9f834e26b29975ed193df9
Deleted: sha256:78dbfa5b7cbc2bd94ccbdba52e71be39b359ed7eac43972891b136334f5ce181
Untagged: hortonworks/cloudbreak:1.16.5
Deleted: sha256:01eab0d1eeb4350e820d599aa9db0baace6653d9a92bcfe305ffb243fb528567
Deleted: sha256:ccb63f10f16b1e5be72886b0d3a8ef08b37fc5620df9b9217a807a16f95c2c38
Deleted: sha256:ba62068aab29eff92419482f5789101ff8a09d084ef4e02131b9d3f2f8fda118
Deleted: sha256:2ed2605493f1ba44cb54783a5e862fa1d7a09e88dd0d4c7ddd7b6f4052b75714
Deleted: sha256:59f6e2912bf6036b57166a02f3054339c88401249c36b968d54e28cd0904f2ad
Deleted: sha256:4baf94f050e5a89cbf0c9e7d9b4f998aac7f6ef9f8b427cab6299f1dd3c7e6b9
Deleted: sha256:7677b042549f18e4b2e97b2679ce2430b20c8acf656faa147d08f4b91a2099c6
Untagged: hortonworks/cloudbreak-autoscale:1.16.5
Deleted: sha256:840e4e087f942ca93dd377cbdeb9f21bb9aef35986e2f689dd1c747ee7c0a21c
Deleted: sha256:ccb0db93a273f01700c4f44f241aa6ac3224da57cc0b0ce0f7998535607eb1e9
Deleted: sha256:bdbb61cbccb4aa121c8b0bbc781e33aff0ad81ec99f7897d33d70b981b15e419
Deleted: sha256:60ffec45fe96f6fbeda751a953e3db103e3241ac856ca898a61168644127da68
Deleted: sha256:3710930b45502b26ca40aa94fb08ef3fdc543d7a7aa08a5da6af0c7841339749
Deleted: sha256:35ec115408fa3e75e10c9c7cfd1e563d587809ad1e1f7865c085837fbfafb2dd
Deleted: sha256:db71d00e76774cc5d1b76ad6f99a6d8f88271e12248fbb338884850ef583f525
[cloudbreak@ip-172-31-15-208 cloudbreak-deployment]$ cbd doctor
===> Deployer doctor: Checks your environment, and reports a diagnose.
uname: Linux ip-172-31-15-208.us-west-2.compute.internal 3.10.0-327.18.2.el7.x86_64 #1 SMP Fri Apr 8 05:09:53 EDT 2016 x86_64 x86_64 x86_64 GNU/Linux
local version:2.4.0
latest release:2.4.0
docker images:
 hortonworks/haveged:1.1.0
 hortonworks/socat:1.0.0
 hortonworks/logspout:v3.2.2
 hortonworks/logrotate:1.0.1
 hortonworks/cbd-smartsense:0.12.0
 hortonworks/cloudbreak-uaa:3.6.5-pgupdate
 hortonworks/cloudbreak:2.4.0
 hortonworks/hdc-auth:2.4.0
 hortonworks/hdc-web:2.4.0
 hortonworks/cloudbreak-autoscale:2.4.0
docker command exists: OK
docker client version: 1.10.3
docker client version: 1.10.3
ping 8.8.8.8 on host: OK
ping github.com on host: OK
ping 8.8.8.8 in container: OK
ping github.com in container: OK
[cloudbreak@ip-172-31-15-208 cloudbreak-deployment]$ cbd start
generating docker-compose.yml
generating uaa.yml
Pulling uluwatu (hortonworks/hdc-web:2.4.0)...
2.4.0: Pulling from hortonworks/hdc-web
Digest: sha256:b6e77035e8290739ffd0224229dc85bf4099b5e2e0efd0bff95e8e440beddea8
Status: Downloaded newer image for hortonworks/hdc-web:2.4.0
Pulling cloudbreak (hortonworks/cloudbreak:2.4.0)...
2.4.0: Pulling from hortonworks/cloudbreak
Digest: sha256:e2196cf706147357b044f544e8cc477eaad2096799bde1a26995e4d0115aed4e
Status: Downloaded newer image for hortonworks/cloudbreak:2.4.0
Pulling logrotate (hortonworks/logrotate:1.0.1)...
1.0.1: Pulling from hortonworks/logrotate
Digest: sha256:9ca38ecacf6430b03b91a445c3a9e47f028a3f1c44d006fe9cf2869cc0621717
Status: Downloaded newer image for hortonworks/logrotate:1.0.1
Pulling periscope (hortonworks/cloudbreak-autoscale:2.4.0)...
2.4.0: Pulling from hortonworks/cloudbreak-autoscale
Digest: sha256:c82a356133eae5abfe17300dd3f06c47c29c8c4f4cf9b003c3ed85b06556a13b
Status: Downloaded newer image for hortonworks/cloudbreak-autoscale:2.4.0
Pulling sultans (hortonworks/hdc-auth:2.4.0)...
2.4.0: Pulling from hortonworks/hdc-auth
Digest: sha256:e4634f055cdd8bc2642fc066bfee0b815376bccc6c8c2e308339e7d5e8a695ea
Status: Downloaded newer image for hortonworks/hdc-auth:2.4.0
Pulling logspout (hortonworks/logspout:v3.2.2)...
v3.2.2: Pulling from hortonworks/logspout
Digest: sha256:ed877a41051bb60ac1c2ff8abab841a7f38b1da1e2ff2bb590724010887e835e
Status: Downloaded newer image for hortonworks/logspout:v3.2.2
Pulling smartsense (hortonworks/cbd-smartsense:0.12.0)...
0.12.0: Pulling from hortonworks/cbd-smartsense
Digest: sha256:38161905e0992c6ba7fea03be6ec1ac6537742d59e7a1e932cb5426325ef6ba6
Status: Downloaded newer image for hortonworks/cbd-smartsense:0.12.0
Pulling identity (hortonworks/cloudbreak-uaa:3.6.5-pgupdate)...
3.6.5-pgupdate: Pulling from hortonworks/cloudbreak-uaa
Digest: sha256:a18340d30a4c885a94be40b74003fa31ea3671d604b1939c09607d921ffe74e4
Status: Downloaded newer image for hortonworks/cloudbreak-uaa:3.6.5-pgupdate
Pulling traefik (traefik:v1.3.8-alpine)...
v1.3.8-alpine: Pulling from library/traefik
Digest: sha256:882bd8f818de9437be8d8bf6c6eeddaa03f43ed92173c2bf2cb1017c8007c65b
Status: Downloaded newer image for traefik:v1.3.8-alpine
Creating cbreak_logrotate_1
Creating cbreak_mail_1
Creating cbreak_periscope_1
Creating cbreak_identity_1
Creating cbreak_commondb_1
Creating cbreak_consul_1
Creating cbreak_haveged_1
Creating cbreak_logsink_1
Creating cbreak_uluwatu_1
Creating cbreak_sultans_1
Creating cbreak_smartsense_1
Creating cbreak_cloudbreak_1
Creating cbreak_registrator_1
Creating cbreak_logspout_1
Creating cbreak_traefik_1
Uluwatu (Cloudbreak UI) url:
  https://ec2-54-200-79-120.us-west-2.compute.amazonaws.com
login email:
  dbialek@hortonworks.com
password:
  ****
[cloudbreak@ip-172-31-15-208 cloudbreak-deployment]$ 
</pre>
    
