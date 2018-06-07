# Configuration

The configuration classes create a Manager Factory. This class has all the configuration to build the database connection.

Once there are a large diversity configuration flavors on such as P2P, master/slave, thrift communication, HTTP, etc. The implementation may be different, however, they have a method to return a Manager Factory.

It is recommended that all database driver providers have a properties file to read this startup information. With the following pattern: diana-provider.properties

