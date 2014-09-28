# Shoutcast Overview

SHOUTcast is cross-platform proprietary software for streaming media over
the Internet. The software, developed by Nullsoft, allows digital audio
content, primarily in MP3 or HE-AAC format, to be broadcast to and from
media player software, enabling the creation of Internet radio "stations".

This charm provides a SHOUTcast DNAS server, the base unit for creating your
online radio. Combined with the SHOUTcast AutoDJ subordinate you can achieve
radio automation of a single stream per AutoDJ subordinate related to this
charm.

# Usage

A typical stand-alone deployment is done by deploying the charm and setting
the two required configuration variables:

    juju deploy shoutcast
    juju set shoutcast password=itsasecret adminpassword=itsreallysecret

You can then browse to http://ip-address:8000 to log into the web service once
deployed and view documentation on how to connect to your streaming server.

Additionally its possible to group configurations for your deployments in a yaml
representation. Given that we will deploy 2 servers, one named *shoutcast* and
another named *relay* our config yaml would look like the following

    shoutcast:
        password: mysecret
        adminpassword: itsnotreallyasecret
    relay:
        password: yoursecret
        adminpassowrd: itsnotreallyasecret

then you can deploy your services specifying the config yaml

    juju deploy shoutcast --config sample.yaml
    juju deploy shoutcast relay --config sample.yaml


# Scale out scenarios

You may wish to setup a relay host so you can support more than the default 512
listeners on a given stream. You can achieve having as many 'relay' hosts as you
desire by adding additional units and relating them with the relay, and source
roles respectively.

    juju deploy shoutcast
    juju deploy shoutcast relay
    juju add-relation shoutcast:scsource relay:screlay


## Known Limitations and Issues

Presently the charm only supports single stream server deployment. Additional
stream configuration, and custom mount point listing will be planned in the
future.

The stream source is only protected by the single password listed in the
configuration of the charm. Multi-user scenarios are expected to be used with
a shared-password model.


#### If you didnt specify passwords during deployment

The current expectation of the charm is to call start once. After you've
updated the charm with `password` and `adminpassword` you will need to
kick the shoutcast service on manually via `juju run`, as calling start
from config-changed could potentially interrupt a live broadcast.

    juju run --service shoutcast "service shoutcast start"

#### What gives, I setup/removed a relay and its not showing up/going away

See the instructions above about recycling the shoutcast process, as we value
the live-stream more than we value recycling your service.

    juju run --service relay "service shoutcast restart"


# Contact Information

Charm Maintainer: Charles Butler <charles.butler@ubuntu.com>

## Upstream Project Name

- Upstream website: http://shoutcast.com
- Upstream bug tracker: http://bugs.launchpad.net/charms/+source/shoutcast
