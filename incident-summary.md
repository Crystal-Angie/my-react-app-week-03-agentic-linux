# Nginx Incident Summary
**Full Name:** Angela C. Chibuike
**Date:** 15/09/2026

## 1. Reported Symptom
Nginx appeared to have stopped either manually or  because the server was experiencing resource pressure. The triage report showed
that Nginx had stopped and needed to recover.

## 2. Evidence Collected
The failed Bash checks showed;
[FAIL] Nginx service is not active
[FAIL] Port 80 is not listening
[FAIL] Local HTTP check returned status 000(connection refused)
[WARN] Root disk usage is 89%
The Nginx logs also showed that the service stopped at 15:47:08T

## 3. Most Likely Cause
From the collected evidence, the most likely cause was low memory arising from the limited available disk space and memory.

Note: While low memory was inferred as the likely cause given the limited resources available arising from the AWS t3.medium instance
in use - running React application, dependencies, Claude, and other system processes-The Nginx outage was caused by the manual
stopping of the Nginx service for incident simulation.
Thus, it is advisable to use t3.large when carrying out this project for more accurate analysis.
This further demonstrates why human oversight matters in agentic systems.And also, the importance of instance sizing as an 
operational consideration.

## 4. Human-Approved Recovery Action
The recovery commands were reviewed before being executed manually. The disk and memory status was checked with:

du -sh /* | sort -rh && free -h

The system was then cleaned up to reduce disk usage and improve available resources.
After which, I restarted nginx with;
sudo systemctl restart Nginx

## 5. Verification
The recovery was confirmed by the following results:
systemctl is-active nginx = active
curl -I http://localhost = 200 OK

These outputs showed that both Nginx and the web application were accessible again.

## 6. Safety Decision
The AI skill was allowed only the read and inspect tools to gather and analyze evidence because these actions were read-only
and did not change the server.
It was not allowed to restart Nginx because restarting a service can affect the running application. Thus strictly implementing
actions requiring human review and manual approval served as safety nets in this project.

## 7. Agentic Loop Mapping
The incident followed the agentic loop:

Gather → Analyze → Human Act → Verify

Gather: The skill collected Nginx, disk, memory, HTTP, and log information.
Analyze: It identified that Nginx had been stopped and that the server had limited available resources.
Human Act: The recovery action was reviewed and performed manually.
Verify: The checks confirmed that Nginx was active, port 80 was listening, and the application returned HTTP 200.
