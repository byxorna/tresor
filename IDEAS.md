# Ideas and Scratchpad

Collins spiritual replacement. Doesnt replicate all functionality
Hierarchical Assets (dc-room-rack-server-container) - linking
Linking can be out of band, not hard and fast (I.e. background job will process rack locations into links, or containers/vms migrate)

## Assets

Can be anything; physical or intangible. Examples are switch, rack, datacenter, server, container, cluster, pool, blast area, layer2 broadcast domain.
Can they be shortlived? (ttl)
Attributes associated with asset?
Assets have states: unused, spinning up, active, spinning down, problem (maint)
Can there be fewer stauses than collins? What about mesos task states? failed? lost?

## Linking

Can assets have relationships with other assets? (see links)
???does linking only work for has-a, or does it do is-a??? (Pool membership vs role definition)

Create assets with optional links. Links can be established, removed, and updated atomically out of band.
/v1/new/:thing?related=host:web-123
Create assets, and associate them with other assets (i.e. a pool or cluster)
/v1/link/:thing1/:thing2

How does linking codify membership, and how is that rectified with status? i.e. pool ALPHA has allocated and spinning down assets. Is membership all things in ALPHA (that with the config?) or only active things?

## IPAM

Ip allocation backend? or delegate to something else?
Would be simple to implement...
Separate enough to allow external providers?

## Querying

Most questions are either "give me id X", or "what things are in this group".
Can groups be codified into pool or cluster? Is primaryrole/secondaryrole useful for querying?
Should tresor perform all filtering or just return simple queries and have client filter?

Support for searching or not? Perhaps "give me Xs in Y" only. Give me containers in this rack/pool/server/blast area.


## Other

Pluggable backends for storage?
How do you talk about things in roles?



