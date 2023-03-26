# Containers

This repo is intended to store all of the container files I use on my
personal machine.

Due to using Fedora as my OS I'm currently using `podman` rather than
`docker`. But all the `podman` commands should work as expected when
using `docker`.

## apacheWebserver

This directory stores the container file to run a apache webserver, I use this
when updating files within my [personal website repo](https://github.com/nathanberry97/personalWebsite).

As I like to see how my changes will reflect the website on my mobile and pc
before pushing the changes to the repo, where my CI/CD pipeline will automatically
update my AWS infra with new changes to the site.
