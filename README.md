## (CORE) devops/sre.technical-tests.infrastructure-as-code

Given an application with a frontend, a backend and a Redis, write Terraform scripts to provision a necessary infrastructure on AWS. These scripts should include the following resources:
  - 3 EC2 instances `technical-test-ariane`, `technical-test-falcon`, `technical-test-redis`
  - a VPC containing with following attached resources `technical-test-vpc`
    - a VPC Internet gateway `technical-test-internet-gateway`
    - a VPC NAT gateway `technical-test-nat-gateway`
    - 2 subnets (a `technical-test-public` and a `technical-test-private`)
    - an elastic ip address in the `technical-test-public` attached to the EC2 instance `technical-test`
    - a default VPC routing table `technical-test-routing-table`
      - the necessary route table entries to redirect the subnets
    - 3 security-groups
      - a security group `technical-test-ariane-security-group` with the following permission entries, attached to `technical-test-ariane`
        - Inbound/ protocol: HTTPS (TCP), port: 443, source: 82.11.22.33/32, descripton: Office
        - Inbound/ protocol: HTTPS (TCP), port: 443, source: 81.44.55.87/32, descripton: VPN
        - Inbound/ protocol: HTTPS (TCP), port: 443, source: 87.12.33.88/32, descripton: Home
        - Outboud/ protocol: All, port: All, source: 0.0.0.0/0, descripton: Internet outbound
      - a security group `technical-test-falcon-security-group` with the following permission entries, attached to `technical-test-falcon`
        - Inbound/ protocol: HTTP (TCP), port: 4000, source: `technical-test-ariane-security-group`, descripton: `technical-test-ariane-security-group`
        - Outboud/ protocol: All, port: All, source: 0.0.0.0/0, descripton: Internet outbound
      - a security group `technical-test-redis-security-group` with the following permission entries, attached to `technical-test-redis`
        - Inbound/ protocol: HTTP (TCP), port: 6399, source: `technical-test-falcon-security-group`, descripton: `technical-test-falcon-security-group`
        - Outboud/ protocol: All, port: All, source: 0.0.0.0/0, descripton: Internet outbound

Deliverables:

- Terraform scripts including all mentioned resources
- Terraform plan/output file showing resources to be created.
- README file explaining:
  - How to initialize and apply the Terraform configuration.
  - Resource hierarchy and purpose.
