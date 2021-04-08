# SevenBridges
Automate the creation of Docker network topologies.

## Sample YAML file
The following YAML file would be used for creating a network with the topology {{0, 1, 2}, {2, 3}, {1, 3}}.
We also bridge container 1 to the host network.
Each number in the topology represents a container, and each set is a network.
Notice there's a hole in this network (around {1, 2, 3}).
```yaml
name: "example"
image: "alpine"
networks:
  - containers: [0, 1, 2]
    label: "big"
  - containers: [2, 3]
    image: "ubuntu"
  - containers: [1, 3]
containers:
  - bridge: true
    cmd: ["sh"]
    image: "ubuntu"
    containers:
      - 1
```

## Example usage
Create a Mobius strip of alpine containers:
```sh
$ 7b ./SevenBridges/examples/mobiusStrip.yaml
created 6 networks
started 6 containers

components: 1
hole count:
	1-dimensional: 1
	2-dimensional: 0

minimal paths around 1-dimensional holes:
	0->2->1->0
```

Teardown the mobius strip we just created:
```sh
$ 7b -teardown ./SevenBridges/examples/mobiusStrip.yaml
```

## Options
 - `networks.containers`
 - `networks.label`
 - `networks.image`
 - `networks.external`
 - `networks.driver`
 - `networks.enableIPV6`
 - `containers.containers`
 - `containers.cmd`
 - `containers.image`
 - `containers.bridge`
 - `containers.networks`
