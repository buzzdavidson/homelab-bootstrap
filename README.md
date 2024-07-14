# homelab-bootstrap

This project resulted from lessons learned from overly ambitious earlier attempts.  Previous effort attempted to bootstrap everything using a massive Terraform project; it worked, but represented an all-or-nothing approach with all of the requisite problems that come along with Terraform.  This new project utilizes Kestra workflow management to drive the overall process, allowing more fine-grained control and visibility than previously available.

## You just finished the Terraform implementation, why change the process?

The all-or-nothing nature of the previous attempt just wasnt very useful after things were initially set up; I still had ongoing maintenance, upgrade, and monitoring stuff to configure.  I was evaluating Kestra for data pipeline use and realized that it would solve these issues quite handily; it can coordinate initial installation as well as ongoing configuration management.

## A huge warning

Note: keep in mind that no docker containers running within bd-host.buzzdavidson.com can access the host's network!  In other words, we can only run workloads that do not need to communicate directly with TrueNAS - you cannot mount NFS or SMB shares from this host.  This is a resolvable problem, but it requires creating a bridge interface on TrueNAS and a bug in the current TrueNAS version prevents this functionality from working properly.  Ultimately it's a minor inconvenience; arguably, we shouldnt be running apps of that nature on a VM running on TrueNAS anyway.

## Prerequisites

Eventually we will want to automate from a bare trueNAS system for recoverability purposes, but for the sake of time and my sanity, this effort contains the following prerequisites:

* TrueNAS server configured and available
* Virtual machine created and configured with ip 10.40.100.100 and hostname bd-host.buzzdavidson.com
* Docker and docker-compose installed on virtual machine
* virtual machine configured with SSH key for passwordless login and ansible control

Note that this is all pretty straightforward to do with Terraform if we so desire.

## So what does this thing do?

This project installs Kestra on a VM running on the TrueNAS server; Kestra workflows bootstrap everything else.
