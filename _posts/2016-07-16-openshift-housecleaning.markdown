---
layout: post
title:  "OpenShift Housecleaning"
date:   2016-07-16
categories: cloud
---

The other day I upgraded the libraries used on one of my node express data services and also did
some basic code cleanup. When I pushed the update to [OpenShift][openshift] the deployment failed.
It said that there were incompatibilities between the libraries I was using and certain other
libraries I would _never_ use with a node express application, such as [karma][karma].

Then I remembered that this application was at one time a full MEAN stack application,
having both an Angular based client and the node express data service. The [karma][karma]
libraries must be cruft left over from that time. But where is it?

I saw that the error I was getting referred to a log file at ```/var/lib/openshift/{app-id}/app-root/runtime/repo/npm-debug.log```
so I started in that directory, and sure enough, there was a ```node_modules``` directory with
all of the extra cruft that was causing me all the issues, so here is what I did:


```
cd app-root/runtime/repo/node_modules
rm -rf *
cd ..
npm i
cd ../../../
gear deploy master

```

Once the deployment completed, I had a functioning data service once again.


[openshift]: https://www.openshift.com/
[karma]: https://karma-runner.github.io/1.0/index.html