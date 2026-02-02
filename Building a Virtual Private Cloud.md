There is always one Default VPC that AWS provides us.

If your VPC is a city, subnets are like different neighborhoods inside your city. You use subnets to group resources with similar access rules and restrictions. Some subnets might be public areas that all resources can access (public subnets) while others are private areas with limited access (private subnets).

The default VPC in your account comes with predefined subnets in each Availability Zone of a Region, which means you'll see 3 subnets on your page if your Region has 3 Availability Zones.

A public subnet is connected to the internet 🌐 Resources inside a public subnet can communicate with external networks.

A private subnet does not have direct internet access 🔐 You'd use it for internal resources that don’t need to be publicly accessible.
