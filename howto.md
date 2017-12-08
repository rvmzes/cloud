# cloud
google cloud mpi project

## Generate SSH keypair on your local machine

1. Run `ssh-keygen -t rsa`
2. Enter filename for key(eg. <USER>)
3. Enter your password

## Send your public key to the cloud instance(only master)

1. Run `scp <USER>.pub <USER>@<EXTERNAL_IP_OF_FIRST_INSANCE>:/home/<USER>/.ssh`
2. Add it to `authorized_keys`. To do this run `cat .ssh/<USER>.pub >> .ssh/authorized_keys`

## Verify SSH connection

To do this try to SSH into machine `ssh -i <USER> <USER>@<EXTERNAL_IP_OF_FIRST_INSANCE>`.
You should be greated with welcome message.

## Make a copy of the instance

1. Create snapshot based on the instance-1
2. Create instance-2 based on the snapshot.

## Verify SSH connection to instance-2

1. Run `ssh -i <USER> <USER>@<EXTERNAL_IP_OF_SECOND_INSTANCE>`
2. You should see welcome message.

## Copy your private key to instance-1

We need this to be able to run `mpirun` on the instance-1.
1. Run `scp <USER> <USER>@<EXTERNAL_IP_OF_FIRST_INSANCE>:/home/<USER>/.ssh` .
2. Rename key to `id_rsa`. Run `mv .ssh/<USER> .ssh/id_rsa`.
3. To verify that everything works run `ssh <INTERNAL_IP_OF_SECOND_INSTANCE>`.
4. You should see welcome message.

## Create HOSTFILE

We need to provide IP addresses to MPI.

1. Create `hostfile` on instance-1. Run `touch hostfile`.
2. Run `echo <INTERNAL_IP_OF_THE_INSTANCE> >> hostfile` for each machine in the cluster.

## Run MPI

Now you can run your program on the cluster.

```
mpirun -np <NUMBER_OF_PROCESSES> --hostfile <HOSTFILE_NAME> ./<APPLICATION>
```
