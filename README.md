# aws-route53-failover-lab
Configured Route 53 health checks, SNS alarms, and failover A records to achieve high-availability for a simple LAMP café application across two Availability Zones.
Attach Screenshots (please embed each image twice)

    Primary Health Check Created

    Health Check Monitoring (Healthy)

    SNS Subscription Confirmation

    Health Check Monitoring (Unhealthy)

    Primary A Record Created

    Secondary A Record Created

    Record List with Failover

    Verify Primary via DNS

    EC2 – Primary Stopped

    Route 53 Health Check Alarm

    Health Check Monitoring Graph

    Verify Secondary via DNS

    SNS Alarm Notification Email

    CloudWatch Alarm Details

Step-by-Step Lab Execution
Task 1: Confirm Café Websites

    Retrieve Credentials from the lab panel (Show Details).

    Note the values for:

        CafeInstance1IPAddress = 35.81.9.214

        PrimaryWebSiteURL = 35.81.9.214/cafe

        SecondaryWebsiteURL = 54.191.81.4/cafe

        CafeInstance2IPAddress = 54.191.81.4

    In the EC2 console (us-west-2), browse both URLs to confirm the café app loads in AZ us-west-2a and us-west-2b.

    Place a test order via the Menu to verify the order confirmation page.

Task 2: Create a Route 53 Health Check

    In Route 53 → Health checks, click Create health check.

    Configure:

        Name: Primary-Website-Health

        Endpoint by: IP address → 35.81.9.214

        Path: /cafe

        Request interval: Fast (10 s)

        Failure threshold: 2

    Enable SNS alarm:

        New SNS topic: Primary-Website-Health

        Email: your address

    Confirm the Subscription via email.

Task 3: Create Failover DNS Records
3.1 Primary A Record

    Route 53 → Hosted zones → your zone → Create record.

    Set:

        Name: www

        Type: A

        Value: 35.81.9.214

        TTL: 15

        Routing policy: Failover → Primary

        Health check: Primary-Website-Health

        Record ID: FailoverPrimary

3.2 Secondary A Record

    Click Create record again.

    Set:

        Name: www

        Type: A

        Value: 54.191.81.4

        TTL: 15

        Routing policy: Failover → Secondary

        Leave Health check blank

        Record ID: FailoverSecondary

Task 4: Verify DNS Resolution

    Copy the Record name (e.g. www.<your-zone>.vocareum.training).

    In a browser, load

    http://www.<your-zone>.vocareum.training/cafe

    Confirm you see the Primary instance (us-west-2a, 35.81.9.214).

Task 5: Simulate Failover

    Switch AWS region to us-west-2, EC2 → Instances.

    Select CafeInstance1 (35.81.9.214) → Stop instance.

    In Route 53 → Health checks → Monitoring, wait for Unhealthy.

    Refresh the café URL—now the site should come from CafeInstance2 (us-west-2b, 54.191.81.4).

    Check your email for the CloudWatch alarm notification.

Task 6: End the Lab

    Return to the lab panel and click End Lab → Yes.

    Verify the “resources terminating” confirmation.

Congratulations! You have successfully built and tested a highly-available, self-healing DNS failover setup using AWS Route 53.
