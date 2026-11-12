## What it does 
Keeps an identity provider (Authentik) in sync with an HR system (OrangeHRM): new hires get an account and a department group, transfers swap groups, and terminations disable the account and remove its groups.

**What it runs on:** The scripts use `jml_common.py` , a helper module the lab provides. It reads employees from the OrangeHRM database and calls the Authentik API.

**How I built it:** I wrote three small Python scripts: one for new hires, one for people who change departments, and one for people who leave. After running each one, I checked the IAM Lab Dashboard to see if HR and Authentik matched. The Mover script was the hardest because it had to add the person to their new group and also remember to take them out of their old one.

**The bug I fixed:** Access creep it's when someone keeps permissions from a old role after they move to a new one.This is important because over time they build up access they no longer need, which breaks least privilege and makes a bigger risk if their account is compromised. Before TODO 3, the Mover added the user to their department group but never removed the old one, so someone who moved from X-Men to Avengers stayed in both. TODO 3 made the mover remove any department that didn't match HR, so the user ends up in the correct group.

**What I'd improve for a real company:** I would keep a record of every change, like who got an account, who changed groups, and who was turned off, and when it happened. Right now the changes happen but nothing is written down. In a real company, you need that record to prove someone lost access when they left.

**Where I built it:** In a personal lab environment, not in production.

**The Joiner**


<img width="780" height="336" alt="Picture1" src="https://github.com/user-attachments/assets/c17005cf-6b36-4fef-bbfa-bb1eb3723dc3" />

**After the Script was Ran**


<img width="780" height="584" alt="Picture5" src="https://github.com/user-attachments/assets/4e86e7b9-0ec4-4538-a7f4-a6abd5c1d91f" />

**The Mover** 


Notice how it shows Katie still has the access to do her old job because she's still in her old group.

<img width="742" height="287" alt="image" src="https://github.com/user-attachments/assets/d39c0e26-2f0f-418d-9451-833b267790ae" />

**After running the script it shows Katie was removed from dept-avengers.

<img width="780" height="377" alt="Picture11" src="https://github.com/user-attachments/assets/3fb01330-f688-4154-8dad-1ec7bfaa4724" />

**The Leaver**


After terminating the user account it showed "Terminated but still active" and the user still had access to their department groups.

<img width="780" height="344" alt="Picture15" src="https://github.com/user-attachments/assets/10bef383-1f7d-4443-81f4-a420d9f8fbca" />

<img width="780" height="207" alt="Picture16" src="https://github.com/user-attachments/assets/90222022-25b3-4179-a4b9-701078635464" />



**After running the Script**


I was able to disable the users account and also remove access from the department groups.

<img width="475" height="131" alt="image" src="https://github.com/user-attachments/assets/6ec59430-6072-44ec-9909-969fa9161cc1" />

<img width="740" height="262" alt="image" src="https://github.com/user-attachments/assets/eb590a99-5a3a-44cf-af6e-f9988955c6ac" />








