# 5. dnsmasq is used to add dns resolution for internal hostnames

Date: 2018-02-19

## Status

Accepted

## Context

Vagrant provides plugins that help with some aspects of the development lifecycle.  For example, the landrush plugin automatically wires in DNS resolution of your vagrant multi-vm environment.  However, landrush also causes some issues: (1) it breaks external DNS resolution to the likes of www.google.com. (2) it simplifies the setup of virtualbox at the expense of infrastructure-as-code:  The landrush DNS configuration clearly is not rolled out onto your test or production infrastructure.

## Decision

Because of the above problems, it was decided to use dnsmasq to configure internal DNS because (1) internal and external DNS resolution is retained and (2) the dnsmasq is true to the infrastructure-as-code spirit in that it can also be rolled out onto test and production environment; whereas landrush cannot.

## Consequences

See above for disussion
