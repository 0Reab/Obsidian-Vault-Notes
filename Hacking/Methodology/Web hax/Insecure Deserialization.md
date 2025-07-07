In high level overview.
Crafting a special cookie and setting it, can lead to a full server compromise.

The mechanism of it is the deserialization of the cookie is vulnerable.
Usually Python is much easier to exploit and other languages such as PHP or java are harder.

### Serialization and ...

Is a process in which data is encoded in binary.

For example a JSON object of some user data can be `serialized` for storage into binary.
And vice versa for reading that same object we can `deserialize` the stored data and read the JSON.

While this process is just encoding into binary, and vice versa.
It's not the only way it's done and the basic idea is transforming data into a medium suitable for transport and storage.
And often that can be bytes.

