# Failed-login investigation with Wazuh

## Setup

I used a Wazuh 4.12 all-in-one VM and a Windows 10 test VM. From my host machine, I made about 15 unsuccessful RDP login attempts over roughly two minutes, using different test usernames.

This was a local simulation. It was not an attack against a public server.

## What I observed

The Windows agent collected Security events through `eventchannel`. In Wazuh Discover, I found Event ID 4625 records around the test period, with NTLM authentication and logon type 3.

Those records show failed network authentication. The timing matched the RDP test, but event 4625 and logon type 3 alone do not prove an RDP brute-force attack. I did not create or validate a custom correlation rule, and no blocking response ran.

## What I learned

The useful part was getting the Windows Security channel collected correctly and checking the event fields instead of relying only on an alert title. An agent query filter can hide the events you expect to see.

To extend the exercise, I would add a documented correlation rule, replay both benign and repeated-failure examples, and check whether it distinguishes them. Account lockout and restricted RDP access would be separate controls to test.

The original screenshot included local addresses and machine information, so I removed it from the current version. I have not recreated the lab for this cleanup.
