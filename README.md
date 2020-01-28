# k8s-cilium-remote-sandbox
k8s cilium  eBPF microservice API network visibility WAF


~~~~

vagrant up



vagrant global-status
id       name            provider   state    directory
-----------------------------------------------------------------------------------------------------------
c34c93c  k8s-master01    virtualbox running  C:/multimachine/kubernetes-sandbox-remote
adb4ffe  worker01        virtualbox running  C:/multimachine/kubernetes-sandbox-remote
2e21187  worker02        virtualbox running  C:/multimachine/kubernetes-sandbox-remote
b39b49d  remotecontrol01 virtualbox running  C:/multimachine/kubernetes-sandbox-remote

vagrant ssh remotecontrol01

vagrant@remotecontrol01:~$ sudo ansible-playbook -i /vagrant/kube-cluster/hosts /vagrant/kube-cluster/1_initial.yml
vagrant@remotecontrol01:~$ sudo ansible-playbook -i /vagrant/kube-cluster/hosts /vagrant/kube-cluster/2_kube-dependencies.yml
vagrant@remotecontrol01:~$ sudo ansible-playbook -i /vagrant/kube-cluster/hosts /vagrant/kube-cluster/3_masters.yml
vagrant@remotecontrol01:~$ sudo ansible-playbook -i /vagrant/kube-cluster/hosts /vagrant/kube-cluster/4_workers.yml
vagrant@remotecontrol01:~$ sudo ansible-playbook -i /vagrant/kube-cluster/hosts /vagrant/kube-cluster/5_clients.yml
~~~~



~~~~
##AGE!!! resources
vagrant@k8s-master01:~$ kubectl get nodes
NAME           STATUS   ROLES    AGE     VERSION
k8s-master01   Ready    master   10m     v1.15.2
worker01       Ready    <none>   9m47s   v1.15.2
worker02       Ready    <none>   9m48s   v1.15.2

vagrant@k8s-master01:~$ kubectl get pods --all-namespaces
NAMESPACE     NAME                                   READY   STATUS    RESTARTS   AGE
kube-system   cilium-etcd-grt4ttk8tt                 1/1     Running   0          2m54s
kube-system   cilium-etcd-jk665w58sp                 1/1     Running   0          102s
kube-system   cilium-etcd-operator-8c775864b-9pq8r   1/1     Running   0          16m
kube-system   cilium-etcd-s6kwwgf27q                 1/1     Running   0          46s
kube-system   cilium-fj7xt                           1/1     Running   1          16m
kube-system   cilium-gxw2r                           1/1     Running   1          16m
kube-system   cilium-operator-6989cf54dc-5dp5q       1/1     Running   1          16m
kube-system   cilium-zgpct                           1/1     Running   2          16m
kube-system   coredns-5c98db65d4-4b9mz               1/1     Running   0          16m
kube-system   coredns-5c98db65d4-kwz28               1/1     Running   0          16m
kube-system   etcd-k8s-master01                      1/1     Running   0          15m
kube-system   etcd-operator-86ccfd897-hdcfj          1/1     Running   0          4m41s
kube-system   kube-apiserver-k8s-master01            1/1     Running   0          15m
kube-system   kube-controller-manager-k8s-master01   1/1     Running   1          15m
kube-system   kube-proxy-678n2                       1/1     Running   0          16m
kube-system   kube-proxy-mwkgf                       1/1     Running   0          16m
kube-system   kube-proxy-snnb9                       1/1     Running   0          16m
kube-system   kube-scheduler-k8s-master01            1/1     Running   1          15m

vagrant@k8s-master01:~$ kubectl get pods -n kube-system --selector=k8s-app=cilium
NAME           READY   STATUS    RESTARTS   AGE
cilium-fj7xt   1/1     Running   1          17m
cilium-gxw2r   1/1     Running   1          17m
cilium-zgpct   1/1     Running   2          17m
~~~~
~~~~
(do not bother kernel upgrading)
  Operating System: Ubuntu 19.04
            Kernel: Linux 5.0.0-17-generic
 NOT OK: minimal supported kernel version is >= 4.8.0; kernel version that is running is: 4.4.0" subsys=daemon

 System Requirements
 https://docs.cilium.io/en/stable/install/system_requirements/#admin-system-reqs

 $ kubectl --namespace kube-system logs cilium-813gf
 NOT OK: minimal supported kernel version is >= 4.8
 https://docs.cilium.io/en/stable/kubernetes/troubleshooting/#verifying-the-installation
 ~~~~
 ~~~~
 Cilium can be integrated with Docker in two ways:

via the CNI interface. This method is used by Kubernetes and Mesos.
via Docker’s libnetwork plugin interface, if networking is to be managed by the Docker runtime. This method is used, for example, by Docker Compose.
https://docs.cilium.io/en/stable/docker/
 ~~~~
 ~~~~
v1.15 Release Notes
The list of validated docker versions remains unchanged.
The current list is 1.13.1, 17.03, 17.06, 17.09, 18.06, 18.09. (#72823, #72831)
https://kubernetes.io/docs/setup/release/notes/

Container runtimes
On each of your machines, install Docker. Version 18.06.2 is recommended, but 1.11, 1.12, 1.13, 17.03 and 18.09 are known to work as well. Keep track of the latest verified Docker version in the Kubernetes release notes.
https://kubernetes.io/docs/setup/production-environment/container-runtimes/

[WARNING IsDockerSystemdCheck]: detected "cgroupfs" as the Docker cgroup driver. The recommended driver is "systemd". Please follow the guide at https://kubernetes.io/docs/setup/cri/

~~~~
~~~~
vagrant@k8s-master01:~$ kubectl get nodes
NAME           STATUS   ROLES    AGE   VERSION
k8s-master01   Ready    master   14m   v1.15.2
worker01       Ready    <none>   11m   v1.15.2
worker02       Ready    <none>   11m   v1.15.2
vagrant@k8s-master01:~$ kubectl get pods --all-namespaces
          1/1     Running   0          48m
~~~~


~~~~
How to Secure a Cassandra Database
https://docs.cilium.io/en/stable/gettingstarted/cassandra/#gs-cassandra

Kubernetes will deploy the pods and service in the background.
$ kubectl create -f https://raw.githubusercontent.com/cilium/cilium/1.5.5/examples/kubernetes-cassandra/cass-sw-app.yaml
deployment.extensions/cass-server created
service/cassandra-svc created
deployment.extensions/empire-hq created
deployment.extensions/empire-outpost created


the progress of the operation
$ kubectl get svc,pods
NAME                    TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)    AGE
service/cassandra-svc   ClusterIP   None         <none>        9042/TCP   16s
service/kubernetes      ClusterIP   10.96.0.1    <none>        443/TCP    79m

NAME                                 READY   STATUS              RESTARTS   AGE
pod/cass-server-5898ffd7b8-pbnr8     0/1     ContainerCreating   0          16s
pod/empire-hq-69b844cdd6-4cqfn       0/1     ContainerCreating   0          16s
pod/empire-outpost-b6c84d55d-b24cr   0/1     ContainerCreating   0          16s
$ kubectl get svc,pods
NAME                    TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)    AGE
service/cassandra-svc   ClusterIP   None         <none>        9042/TCP   2m21s
service/kubernetes      ClusterIP   10.96.0.1    <none>        443/TCP    81m

NAME                                 READY   STATUS    RESTARTS   AGE
pod/cass-server-5898ffd7b8-pbnr8     1/1     Running   0          2m21s
pod/empire-hq-69b844cdd6-4cqfn       1/1     Running   0          2m21s
pod/empire-outpost-b6c84d55d-b24cr   1/1     Running   0          2m21s


Test Basic Cassandra Access
create the keyspaces and tables and populate them with some initial data:
$ curl -s https://raw.githubusercontent.com/cilium/cilium/1.5.5/examples/kubernetes-cassandra/cass-populate-tables.sh | bash

create two environment variables that refer to the empire-hq and empire-outpost pods
vagrant@k8s-master01:~$ HQ_POD=$(kubectl get pods -l app=empire-hq -o jsonpath='{.items[0].metadata.name}')
vagrant@k8s-master01:~$ OUTPOST_POD=$(kubectl get pods -l app=empire-outpost -o jsonpath='{.items[0].metadata.name}')


run the ‘cqlsh’ Cassandra client in the empire-outpost pod, telling it to access the Cassandra cluster identified by the ‘cassandra-svc’ DNS name
vagrant@k8s-master01:~$ kubectl exec -it $OUTPOST_POD cqlsh -- cassandra-svc
Connected to Test Cluster at cassandra-svc:9042.
[cqlsh 5.0.1 | Cassandra 3.11.4 | CQL spec 3.4.4 | Native protocol v4]
Use HELP for help.
cqlsh>


show that the outpost can add records to the “daily_records” table in the “attendance” keyspace:
cqlsh> INSERT INTO attendance.daily_records (creation, loc_id, present, empire_member_id) values (now(), 074AD3B9-A47D-4EBC-83D3-CAD75B1911CE, true, 6       AD3139F-EBFC-4E0C-9F79-8F997BA01D90);
cqlsh>


every client has access to the Cassandra API on port 9042
the outpost container can not only add entries to the attendance.daily_reports table, but it could read all entries as well.
cqlsh> SELECT * FROM attendance.daily_records;

 loc_id                               | creation                             | empire_member_id                     | present
--------------------------------------+--------------------------------------+--------------------------------------+---------
 074ad3b9-a47d-4ebc-83d3-cad75b1911ce | ab57ded0-bd40-11e9-8931-e7b2f09d3df6 | 6ad3139f-ebfc-4e0c-9f79-8f997ba01d90 |    True
 8218ab55-b0af-4e78-9f7e-b842e05a5cd1 | 3b95f230-bd40-11e9-8931-e7b2f09d3df6 | 0cbef8fd-7394-44df-8eb0-0718d9ad288c |    True
 4278f176-9483-4701-bb05-db484d595f0e | 3a89b250-bd40-11e9-8931-e7b2f09d3df6 | 539d6827-092d-4aa0-9876-1bff1291278e |    True
 6953ebeb-dcfe-4100-bc70-bbb89752ad08 | 3cbb1140-bd40-11e9-8931-e7b2f09d3df6 | 4d7b71ae-8a46-41e5-86ed-aed5297e95f6 |    True
 78d5db40-982b-4668-9b25-4489389d99f5 | 3e7607b0-bd40-11e9-8931-e7b2f09d3df6 | 26ff7ca8-b23c-48d8-b001-31f62dc1f36b |    True
 e3bbc367-9545-4d5c-90e3-2c7ddb97ef07 | 3d526f40-bd40-11e9-8931-e7b2f09d3df6 | ce80b4b8-e907-47e3-b8f2-7a034c6570ee |    True
 63311a94-ba05-48e6-b682-a99989c584bd | 38c6f3b0-bd40-11e9-8931-e7b2f09d3df6 | 0f58bd23-5715-46ff-8eef-6306859bddd8 |    True
 2ba760f1-a3bf-4f05-9f1d-e1ec27dd4829 | 396a86b0-bd40-11e9-8931-e7b2f09d3df6 | e56c3de1-b745-452f-a71c-5109e29c0bd1 |    True
 6f075f4f-270b-414c-b7d4-8a816f2580b6 | 3c2f7310-bd40-11e9-8931-e7b2f09d3df6 | a63d0a19-3157-495b-8c12-2109836041ab |    True
 262558e9-bbae-4f26-a99f-94787860e141 | 39f95930-bd40-11e9-8931-e7b2f09d3df6 | cc56e530-a675-4af1-aa94-7fc6061ea243 |    True
 d0e25785-2944-479d-8583-8971ad3e3d81 | 3ddb2740-bd40-11e9-8931-e7b2f09d3df6 | 4e383aa7-ea0a-4e52-9409-30f1815de7d7 |    True
 b5202d66-20a5-4bcc-938f-478d3c4dd9b2 | 3b1158e0-bd40-11e9-8931-e7b2f09d3df6 | fdc44537-de49-41e9-b049-9bd98a6597f2 |    True

(12 rows)
cqlsh>



the outpost container can also access information in any keyspace, including the deathstar keyspace
cqlsh> SELECT * FROM deathstar.scrum_notes;

 empire_member_id                     | content                                                                                                        | creation
--------------------------------------+----------------------------------------------------------------------------------------------------------------+--------------------------------------
 d36c93b6-4c22-4ad8-a08d-0a7657447721 |   I think the exhaust port could be vulnerable to a direct hit.  Hope no one finds out about it.  Not blocked. | 35b59500-bd40-11e9-8931-e7b2f09d3df6
 cb05370f-3691-41a7-ae7a-26a83269430b | Designed protective shield for deathstar.  Could be based on nearby moon.  Feature punted to v2.  Not blocked. | 36535ba0-bd40-11e9-8931-e7b2f09d3df6
 e17b714f-b38c-4e4c-a1d7-c5db32996cfd |        Trying to figure out if we should paint it medium grey, light grey, or medium-light grey.  Not blocked. | 3525d820-bd40-11e9-8931-e7b2f09d3df6

(3 rows)
cqlsh>

~~~~
Securing Access to Cassandra with Cilium
~~~~
it would be much more secure to limit each pod’s access to the Cassandra server to be least privilege (i.e., only what is needed for the app to operate correctly and nothing more).

with Cilium HTTP policies, write policies that identify pods by labels, and then limit the traffic in/out of this pod.

In this case, create a policy that identifies the tables that each client should be able to access, the actions that are allowed on those tables, and deny the rest.


a policy could limit containers with label app=empire-outpost to only be able to insert entries into the table “attendance.daily_reports”, but would block any attempt by a compromised outpost to read all attendance information or access other keyspaces


Apply this Cassandra-aware network security policy
vagrant@k8s-master01:~$ kubectl create -f https://raw.githubusercontent.com/cilium/cilium/1.5.5/examples/kubernetes-cassandra/cass-sw-security-policy.yaml
ciliumnetworkpolicy.cilium.io/secure-empire-cassandra created


try to perform the attacks from the empire-outpost pod
vagrant@k8s-master01:~$ kubectl exec -it $OUTPOST_POD cqlsh -- cassandra-svc   ium/cilium/1.5.5/examples/kuber
Connected to Test Cluster at cassandra-svc:9042.
[cqlsh 5.0.1 | Cassandra 3.11.4 | CQL spec 3.4.4 | Native protocol v4]
Use HELP for help.
cqlsh> SELECT * FROM attendance.daily_records;
Unauthorized: Error from server: code=2100 [Unauthorized] message="Request Unauthorized"


the outpost container can NOT  access information in any keyspace, including the deathstar keyspace
cqlsh> SELECT * FROM deathstar.scrum_notes;
Unauthorized: Error from server: code=2100 [Unauthorized] message="Request Unauthorized"
cqlsh>


confirm that the empire-hq pod still has full access to the cassandra cluster:
vagrant@k8s-master01:~$ kubectl exec -it $HQ_POD cqlsh -- cassandra-svc
Connected to Test Cluster at cassandra-svc:9042.
[cqlsh 5.0.1 | Cassandra 3.11.4 | CQL spec 3.4.4 | Native protocol v4]
Use HELP for help.
cqlsh>


Cilium’s identity-based security allows empire-hq to still have full access to both tables:
cqlsh> SELECT * FROM attendance.daily_records;

 loc_id                               | creation                             | empire_member_id                     | present
--------------------------------------+--------------------------------------+--------------------------------------+---------
 074ad3b9-a47d-4ebc-83d3-cad75b1911ce | aba306b0-bd42-11e9-8931-e7b2f09d3df6 | 6ad3139f-ebfc-4e0c-9f79-8f997ba01d90 |    True
 8218ab55-b0af-4e78-9f7e-b842e05a5cd1 | 3b95f230-bd40-11e9-8931-e7b2f09d3df6 | 0cbef8fd-7394-44df-8eb0-0718d9ad288c |    True
 4278f176-9483-4701-bb05-db484d595f0e | 3a89b250-bd40-11e9-8931-e7b2f09d3df6 | 539d6827-092d-4aa0-9876-1bff1291278e |    True
 6953ebeb-dcfe-4100-bc70-bbb89752ad08 | 3cbb1140-bd40-11e9-8931-e7b2f09d3df6 | 4d7b71ae-8a46-41e5-86ed-aed5297e95f6 |    True
 78d5db40-982b-4668-9b25-4489389d99f5 | 3e7607b0-bd40-11e9-8931-e7b2f09d3df6 | 26ff7ca8-b23c-48d8-b001-31f62dc1f36b |    True
 e3bbc367-9545-4d5c-90e3-2c7ddb97ef07 | 3d526f40-bd40-11e9-8931-e7b2f09d3df6 | ce80b4b8-e907-47e3-b8f2-7a034c6570ee |    True
 63311a94-ba05-48e6-b682-a99989c584bd | 38c6f3b0-bd40-11e9-8931-e7b2f09d3df6 | 0f58bd23-5715-46ff-8eef-6306859bddd8 |    True
 2ba760f1-a3bf-4f05-9f1d-e1ec27dd4829 | 396a86b0-bd40-11e9-8931-e7b2f09d3df6 | e56c3de1-b745-452f-a71c-5109e29c0bd1 |    True
 6f075f4f-270b-414c-b7d4-8a816f2580b6 | 3c2f7310-bd40-11e9-8931-e7b2f09d3df6 | a63d0a19-3157-495b-8c12-2109836041ab |    True
 262558e9-bbae-4f26-a99f-94787860e141 | 39f95930-bd40-11e9-8931-e7b2f09d3df6 | cc56e530-a675-4af1-aa94-7fc6061ea243 |    True
 d0e25785-2944-479d-8583-8971ad3e3d81 | 3ddb2740-bd40-11e9-8931-e7b2f09d3df6 | 4e383aa7-ea0a-4e52-9409-30f1815de7d7 |    True
 b5202d66-20a5-4bcc-938f-478d3c4dd9b2 | 3b1158e0-bd40-11e9-8931-e7b2f09d3df6 | fdc44537-de49-41e9-b049-9bd98a6597f2 |    True

(12 rows)
cqlsh> SELECT * FROM deathstar.scrum_notes;

 empire_member_id                     | content                                                                                                        | creation
--------------------------------------+----------------------------------------------------------------------------------------------------------------+--------------------------------------
 d36c93b6-4c22-4ad8-a08d-0a7657447721 |   I think the exhaust port could be vulnerable to a direct hit.  Hope no one finds out about it.  Not blocked. | 35b59500-bd40-11e9-8931-e7b2f09d3df6
 cb05370f-3691-41a7-ae7a-26a83269430b | Designed protective shield for deathstar.  Could be based on nearby moon.  Feature punted to v2.  Not blocked. | 36535ba0-bd40-11e9-8931-e7b2f09d3df6
 e17b714f-b38c-4e4c-a1d7-c5db32996cfd |        Trying to figure out if we should paint it medium grey, light grey, or medium-light grey.  Not blocked. | 3525d820-bd40-11e9-8931-e7b2f09d3df6

(3 rows)
cqlsh>
~~~~

Cassandra-Aware Visibility
~~~~
 re-run the above queries with policy enforced and view how Cilium provides Cassandra-aware visibility, including whether requests are forwarded or denied


vagrant@k8s-master01:~$ CILIUM_POD=$(kubectl get pods -n kube-system -l k8s-app=cilium -o jsonpath='{.items[0].metadata.name}')

vagrant@k8s-master01:~$ kubectl get pods -n kube-system -l k8s-app=cilium
NAME           READY   STATUS    RESTARTS   AGE
cilium-fg256   1/1     Running   2          103m
cilium-l6xnh   1/1     Running   0          101m
cilium-vkwpn   1/1     Running   1          101m
vagrant@k8s-master01:~$ kubectl exec -it -n kube-system $CILIUM_POD /bin/bash
root@k8s-master01:~#

start Cilium monitor, and limit the output to only “l7” type messages using the “-t” flag
root@k8s-master01:~# cilium monitor -t l7
Press Ctrl-C to quit


In the other windows, re-run the above queries,
>vagrant ssh k8s-master01

~~~~
