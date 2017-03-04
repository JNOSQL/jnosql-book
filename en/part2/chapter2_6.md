## Configuration



The configuration classes create a Manager Factory. On this class has all configuration to make the database connection.

Once there are a large diversity configuration flavors on such as P2P, master/slave, thrift communication, HTTP, etc. The implementation may be different. However, they have a method to return a Manager Factory.

Is recommended to all databases driver provider to have a properties file to read this startup information. With the pattern: diana-provider.properties



