# Braindump

None of this will make sense.

Collins spiritual replacement. Doesnt replicate all functionality
Hierarchical Assets (dc-room-rack-server-container) - linking
Linking can be out of band, not hard and fast (I.e. background job will process rack locations into links, or containers/vms migrate)

## Assets (things?)

Can be anything; physical or intangible. Examples are switch, rack, datacenter, server, container, cluster, pool, blast area, layer2 broadcast domain.
Can they be shortlived? (ttl)
Attributes associated with asset?
Assets have states: unused, spinning up, active, spinning down, problem (maint)
Can there be fewer stauses than collins? What about mesos task states? failed? lost?

Can an asset actually be a meta asset? Perhaps a "pool" is actually just bound to a query for other assets?

### Attributes

* type - server, switch, pool, blast zone, config. user creatable, can be anything
* id - internal id
* tag - user created tag to identify the asset; unique within that type
* attributes - arbitrary key-value pairs. is this json?

## Linking

*TODO* What does linking gain over normal key:value attributes like collins has? A: key-value doesnt allow the grouping itsself to be managable. You end up talking about either queries or materialized sets of assets (out of date), instead of passing around the grouping object (pool/cluster).

Things have links. Some are heirarchical (has-a), some represent membership (is-a? part-of). A link between a VM and the server hosting it is a good example. The link may change atomically (live vm migration), or become broken for a time (pause VM, move to cold storage, bring up elsewhere).

Create assets with optional links. Links can be established, removed, and updated atomically out of band.
/v1/new/:thing?related=host:web-123
Create assets, and associate them with other assets (i.e. a pool or cluster)
/v1/link/:thing1/:thing2

How does linking codify membership, and how is that rectified with status? i.e. pool ALPHA has allocated and spinning down assets. Is membership all things in ALPHA (that with the config?) or only active things?

### Link Types

Any difference between heirarchical links and membership links? Server is in a rack, container is on a server. Does a container have an explicit link to a rack, or only a server? It would be expensive to traverse all links to find membership? Links should be maintained separate from assets.

### Links and status

What happens to a link when a thing thats linked to goes away? Server goes into spinning down, do containers loose membership in clusters? Does spinning down status transfer to all downstream linked items? (that is a horrible idea, because linking can represent physical connection but not dependence. i.e. im on a switch, but a switch going into problem doesnt mean i stop serving traffic necessarily).

### Upstream/downstream status

A thing can have a status (i.e. problem). Can this effect upstream or downstream links? ("an upstream thing is under maintenance/spinning down"). When you change status can you specify if a status should transfer to other items? How do you represent upstream degraded on an asset?


## IPAM

Ip allocation backend? or delegate to something else?
Would be simple to implement...
Separate enough to allow external providers?

Could this be a plugin?

## Querying

Most questions are either "give me id X", or "what things are in this thing".
Should it be very reliant on the link? I.e. a cluster thing has servers, servers have containers. Can you ask "give me containers in cluster X"?
Can groups be codified into pool or cluster? Is primaryrole/secondaryrole useful for querying?
Should tresor perform all filtering or just return simple queries and have client filter them down?
Support for searching or not? Perhaps "give me Xs in Y" only. Give me containers in this rack/pool/server/blast area.
  -> how do you handle nesting assets? do you need to traverse all things linked to find their links to traverse them?

## Plugins

Allow interesting, non-core implementations? For example, IPAM could be a plugin, with its own namespace and just register as a handler.
Plugins could work for specific types of assets, and implement their own business logic without cluttering the core of Tresor. A LSHW plugin could define a minimum schema lshw reports conform to, and performs normalization and storage. Plugins would need to be able to associate some storage with an asset, so querying for an asset could return desired data.

* IPAM
* LSHW
* LLDP
* IPMI

## Other

Pluggable backends for storage?
How do you talk about things in roles?



