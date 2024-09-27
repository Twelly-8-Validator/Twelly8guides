# Ojo
Here’s the guide for launching a node for the Ojo project based on the official documentation:

### Guide to Launching an Ojo Node

#### Step 1: Install Required Dependencies

Before you start the installation, ensure you have the following dependencies installed:

- **Go** (version 1.20 or higher)
- **Make** (for building the project)
- **Docker** (for using containers)

#### Step 2: Clone the Repository

Clone the Ojo repository:

```bash
git clone https://github.com/ojo-network/ojo.git
cd ojo
```

#### Step 3: Build the Project

Navigate to the project directory and execute the build command:

```bash
make build
```

After a successful build, you will find the executable file in the `bin` directory.

#### Step 4: Start the Node

To start a full node, use the following command:

```bash
./bin/ojo start
```

This command will launch the node, which will begin synchronizing with the Agamotto network.

#### Step 5: Check Node Status

You can check the status of the node by executing:

```bash
./bin/ojo status
```

#### Step 6: View Node Logs

To view the logs, run:

```bash
tail -f ~/.ojo/logs/ojo.log
```

This will allow you to monitor the node's operation in real time.
