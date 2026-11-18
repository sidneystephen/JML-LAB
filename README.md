# JML Lab: Joiner, Mover, Leaver

I'm still learning identity and access management, and this lab was about **JML**, which stands for **Joiner, Mover, Leaver**. Those are the three big moments in someone's time at a company:

- **Joiner:** a new hire starts and needs an account.
- **Mover:** someone changes departments and needs different access.
- **Leaver:** someone leaves and their access needs to be shut off.

The goal was to keep Authentik (the identity provider, where people log in) in sync with OrangeHRM (the HR system, which knows who works where).

## What the Lab Gave Me

The lab provided the Python scripts (`joiner.py`, `mover.py` and `leaver.py`) and a helper module called `jml_common.py`. The helper reads employees from the OrangeHRM database and talks to the Authentik API.

Each script had **TODO** lines left blank. My job was to fill them in (TODO 1 through 7), run each script, and then check the **IAM Lab Dashboard** to see if HR and Authentik matched.

I built this in a personal lab environment, not in production.

---

## 1. The Joiner

**What it does:** Anyone active in HR who doesn't have an account yet gets one, and gets added to their department's group.

**What I filled in:**

- **TODO 1:** Create the account with `ak_create_user(emp)`.
- **TODO 2:** Add them to their department group with `ak_add_to_group(user, dept_group(emp["department"]))`.

**Before:**

<img width="780" height="336" alt="Joiner before" src="https://github.com/user-attachments/assets/c17005cf-6b36-4fef-bbfa-bb1eb3723dc3" />

**After running the script:**

<img width="780" height="584" alt="Joiner after" src="https://github.com/user-attachments/assets/4e86e7b9-0ec4-4538-a7f4-a6abd5c1d91f" />

## 2. The Mover

**What it does:** When someone changes departments, they get added to their new group *and* removed from their old one.

The script already added people to their new group, but it never took away the old one. That causes something called **access creep**: someone keeps permissions from an old role after they move. Over time they build up access they don't need anymore. That breaks **least privilege** (only giving people what they need) and makes it riskier if their account ever gets hacked.

**What I filled in:**

- **TODO 3:** Loop through the user's current groups and remove any group that doesn't match their department in HR.

This was the hardest part for me, because I had to think about taking access away, not just giving it.

**Before:** Katie still had access to her old job because she was still in her old group.

<img width="742" height="287" alt="Mover before" src="https://github.com/user-attachments/assets/d39c0e26-2f0f-418d-9451-833b267790ae" />

**After running the script:** Katie was removed from `dept-avengers`, so she's only in the group that matches HR.

<img width="780" height="377" alt="Mover after" src="https://github.com/user-attachments/assets/3fb01330-f688-4154-8dad-1ec7bfaa4724" />

## 3. The Leaver

**What it does:** Anyone marked as terminated in HR gets their account turned off and loses all their department groups.

**What I filled in:**

- **TODO 4:** Skip anyone who isn't terminated.
- **TODO 5:** Find their account, and skip them if they don't have one.
- **TODO 6:** If the account is still active, turn it off with `ak_set_active(user, False)`.
- **TODO 7:** Remove them from every group they're in.

**Before:** After terminating the user in HR, the dashboard showed **"Terminated but still active,"** and the user still had their department groups.

<img width="780" height="344" alt="Leaver before" src="https://github.com/user-attachments/assets/10bef383-1f7d-4443-81f4-a420d9f8fbca" />

<img width="780" height="207" alt="Leaver before groups" src="https://github.com/user-attachments/assets/90222022-25b3-4179-a4b9-701078635464" />

**After running the script:** The account was disabled and removed from its department groups.

<img width="475" height="131" alt="Leaver after" src="https://github.com/user-attachments/assets/6ec59430-6072-44ec-9909-969fa9161cc1" />

<img width="740" height="262" alt="Leaver after groups" src="https://github.com/user-attachments/assets/eb590a99-5a3a-44cf-af6e-f9988955c6ac" />

---

## What I Learned

- **HR should drive access.** When HR changes, the accounts and groups should follow.
- **Taking access away matters as much as giving it.** The Mover showed me how easy it is to forget the "remove" step.
- **Disabling an account isn't enough by itself.** The Leaver also has to remove group memberships.
- **Always check the result.** The dashboard made it easy to see whether HR and Authentik actually matched.

## What I'd Improve for a Real Company

I'd keep a record of every change: who got an account, who changed groups, who was turned off, and when. Right now the changes happen, but nothing gets written down. A real company needs that record to prove someone lost access when they left.
