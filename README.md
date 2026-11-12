## What it does 
Keeps an identity provider (Authentik) in sync with an HR system (OrangeHRM): new hires get an account and a department group, transfers swap groups, and terminations disable the account and remove its groups.

**What it runs on:** The scripts use `jml_common.py` , a helper module the lab provides. It reads employees from the OrangeHRM database and calls the Authentik API.

**How I built it:** I wrote three small Python scripts: one for new hires, one for people who change departments, and one for people who leave. After running each one, I checked the IAM Lab Dashboard to see if HR and Authentik matched. The Mover script was the hardest because it had to add the person to their new group and also remember to take them out of their old one.

**The bug I fixed:** Access creep it's when someone keeps permissions from a old role after they move to a new one.This is important because over time they build up access they no longer need, which breaks least privilege and makes a bigger risk if their account is compromised. Before TODO 3, the Mover added the user to their department group but never removed the old one, so someone who moved from X-Men to Avengers stayed in both. TODO 3 made the mover remove any department that didn't match HR, so the user ends up in the correct group.

**What I'd improve for a real company:** I would keep a record of every change, like who got an account, who changed groups, and who was turned off, and when it happened. Right now the changes happen but nothing is written down. In a real company, you need that record to prove someone lost access when they left.

**Where I built it:** In a personal lab environment, not in production.
