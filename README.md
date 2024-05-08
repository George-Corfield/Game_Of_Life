# Conways Game of Life: Computer Systems A

## Coursework Description

This coursework project consists of two main parts implemented in Golang. The first part involves a parallel implementation of Conway's Game of Life, utilising Go routines to calculate next states by splitting the board. The second part focuses on a distributed implementation, employing AWS nodes and Remote Procedure Calls (RPCs) to calculate next states by sending portions of the boards.

## Features

### Parallel Implementation of Conway's Game of Life
- Utilises Go routines to parallelise the computation of next states.
- Implements efficient splitting of the board to distribute workload among Go routines.

### Distributed Implementation with AWS Nodes
- Utilizes AWS nodes for distributed computation.
- Utilizes RPCs for communication between nodes.
- Implements fault tolerance mechanisms to ensure the program continues running in case of AWS node failures.
  
### Extensions
- **Fault Tolerance:** Implements mechanisms to handle AWS node failures gracefully, ensuring the continued execution of the program.
- **Halo Regions Calculation:** Extends the functionality to calculate halo regions to improve boundary conditions handling.
- **Parallel Features in Distributed System:** Adds parallel features within the distributed system to enhance performance.

## Usage
To run the coursework, follow the instructions below:

When running the any implementation these flags may be specified and changed:
```bash
--t=threadCount
--w=ImageWidth
--h=ImageHeight
--turns=numberOfTurns
--noVis=disable SDL
```

1. **Parallel Implementation:**
   - Navigate to the [Parallel Implementation](./Parallel).
   - To test the code run `go test .` in the terminal
   - To run the code use `go run .` in the terminal to run the code
   - p = Pause, s = Save, q = Quit

2. **Distributed Implementation on localhost:**
   - Navigate to the [Distributed Implementation](./Distributed) or the [Distributed Extenstion](./Distributed-Extension).
   - open 6 terminal windown. On each of the first 4 run the corresponding line:
```
go run Server/server.go --port=8050
go run Server/server.go --port=8051
go run Server/server.go --port=8053
go run Server/server.go --port=8054
```
   - on the 5th window run `go run Broker/broker.go`
   - Now to test the program run `go test .` or to run it `go run .`
   - If running the Extension version there is an optional flag called continue which allows users to continue from their previous game of life if they had previously quit. This can be accessed via `go run . --broker=... --continue=true`
   - p = Pause, q = Quit, s = Save, k = Kill all Instances

3. **Distributed Implementation on AWS:**
   - Navigate to the [Distributed Implementation](./Distributed) or the [Distributed Extenstion](./Distributed-Extension).
   - Set up 5 AWS EC2 instances
   - SSH into each instance and run the following:
```bash
sudo yum install go
echo 'export GOPATH="$HOME/go"' >> ~/.bashrc
echo 'export PATH="$PATH:/usr/local/go/bin:$GOPATH/bin"' >> ~/.bashrc
source ~/.bashrc
```
   - On each clone the github repository and navigate to the required repository
   - On 4 of the instances run:
```
go run Server/server.go --port=8050
go run Server/server.go --port=8051
go run Server/server.go --port=8053
go run Server/server.go --port=8054
```

   - On the 5th instance run `go run Broker/broker.go --worker1= ... --worker2= ... --worker3= ... --worker4= ...` where `...` represents the ip:port combination for example if your first worker is running on ip address `3.142.56.7` you would have `--worker1=3.142.56.7:8050` etc
   - Finally, on your local machine run `go run . --broker= ...` where `...` represents the ip:port of the Broker/5th instance and the port is 8030
   - If running the Extension version there is an optional flag called continue which allows users to continue from their previous game of life if they had previously quit. This can be accessed via `go run . --broker=... --continue=true`
   - p = Pause, q = Quit, s = Save, k = Kill all Instances
