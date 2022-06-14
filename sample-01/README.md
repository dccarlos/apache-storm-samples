# Setting up the storm sample

> This project is based on the samples provided in
> the [repo](https://github.com/PacktPublishing/Mastering-Apache-Storm)
> from Ankit Jain's `Mastering Apache Storm` book

## Requirements

### Resources

A `linux/x86_64` based PC (non `Apple Silicon`)

1. 2 CPU cores
2. 2 RAM GB
3. 1 GB Swap

### SDKs & apps

1. Java 8
2. Maven
3. Docker (in this sample Docker version `19.03.13` was used)
4. `docker-compose` (in older versions it isn't bundled)

## How to set up the storm docker stack

### Step 1. Open a dedicated terminal

Open a terminal that isn't going to be used later

```shell
cd <this-project>/docker
docker-compose up
```

Wait until you are able to see the storm ui in http://localhost

## How to set up the topology

### Step 1. Define a directory

Define a directory where generated topology artifacts will be set

### Step 2. Define environmental variable

Define environmental variable `STORM_TOPOLOGIES_DIR` with the directory defined in step 1 as value

```shell
export STORM_TOPOLOGIES_DIR=<my-dir>
```

### Step 3. Execute Maven

Execute a maven install (not package, since in install phase the files are copied
to `STORM_TOPOLOGIES_DIR`)

```shell
mvn clean install -DskipTests
```

If everything went well you'll see `storm_example-topology-0.0.1-SNAPSHOT.jar`
in `STORM_TOPOLOGIES_DIR`

## How to deploy the topology in the cluster

### Step 1. Transfer topology to the container

```shell
docker cp <your-storm-topology-dir>/sample-01-topology-0.0.1-SNAPSHOT.jar s02-nimbus:/storm_example-topology.jar
```

### Step 2. Deploy the topology from the container

```shell
docker exec -it s02-nimbus storm jar /storm_example-topology.jar com.stormadvance.storm_example.SampleStormClusterTopology storm_example
```
