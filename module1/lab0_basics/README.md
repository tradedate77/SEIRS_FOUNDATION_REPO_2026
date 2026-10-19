# SEIR Foundations — Module 1, Lab 0

## Your First EC2 Deployment: Launch, Connect, Observe, Repeat

**Course:** SEIR Foundations
**Lab:** Module 1 / Lab 0 — EC2 Basics
**Student submission:** Terelle (`tradedate77`)
**Homework deadline:** Tuesday, October 20, 2026, at 12:00 PM Central Time

---

## 1. Homework summary

For this assignment I performed **three separate EC2 deployments** of Amazon Linux 2023
instances in the **us-east-2** (Ohio) Region, reusing a saved launch template each
time. For each deployment I launched the instance, waited for it to become ready,
recorded its public IPv4 address, connected through **EC2 Instance Connect**, and
verified the SEIR Foundations system dashboard. After collecting evidence, I
terminated each instance.

The three deployments produced **three distinct public IPv4 addresses**, as the lab
requires. All three were `t3.micro` instances running in availability zone
`us-east-2b`.

---

## 2. Homework evidence table

| Deployment | Public IPv4 address | Connection method | Screenshot | Instance terminated? |
|---|---|---|---|---|
| 1 | `3.139.76.105` | EC2 Instance Connect | `screenshots/deployment-1.png` | Yes |
| 2 | `18.117.121.47` | EC2 Instance Connect | `screenshots/deployment-2.png` | Yes |
| 3 | `3.145.138.109` | EC2 Instance Connect | `screenshots/deployment-3.png` | Yes |

**Distinct addresses confirmed:** `3.139.76.105`, `18.117.121.47`, and
`3.145.138.109` are three different public IPv4 addresses. No Elastic IP addresses
were purchased to satisfy this requirement, and AWS did not reuse an address across
the three launches.

### Instance details

| Deployment | Instance ID | Instance type | Region / AZ | Instance name |
|---|---|---|---|---|
| 1 | `i-0b3791672b503f379` | `t3.micro` | us-east-2 / us-east-2b | (name not set) |
| 2 | `i-09083727ba3393a25` | `t3.micro` | us-east-2 / us-east-2b | `Terelle_Homework_2` |
| 3 | `i-011541bc1579afafe` | `t3.micro` | us-east-2 / us-east-2b | `Terelle_Homework_3` |

---

## 3. Deployment write-ups

### Deployment 1 — `3.139.76.105`

I launched the instance from my saved launch template, using Amazon Linux 2023 and a
`t3.micro` instance type. I waited until the instance reached the **Running** state
and passed both status checks, then recorded its public IPv4 address,
`3.139.76.105`, from the instance details page. I connected through **EC2 Instance
Connect** in the browser, which opened a terminal as the `ec2-user` user on the
host. The **SEIR Foundations dashboard** displayed automatically on login, showing
the hostname, uptime, CPU, load average, memory, root disk, instance ID, instance
type, and AWS zone. I captured the screenshot in `screenshots/deployment-1.png`,
which shows the dashboard and the `ec2-user@ip-172-31-31-112` prompt confirming
that I was logged in to a real Linux server. I then terminated the instance.

### Deployment 2 — `18.117.121.47`

The second deployment followed the same sequence, and I named this instance
`Terelle_Homework_2` in the launch wizard so it would be easy to identify in the
console. After the instance passed its status checks, I recorded the public IPv4
address `18.117.121.47` and connected through **EC2 Instance Connect**. The SEIR
dashboard appeared on login, reporting 2 vCPU, 913 MiB of memory, and an 8.0G root
disk. The prompt `ec2-user@ip-172-31-30-249` confirmed the successful connection.
Evidence is in `screenshots/deployment-2.png`. I terminated the instance after
collecting the screenshot.

### Deployment 3 — `3.145.138.109`

For the third deployment I again launched from the saved template and named the
instance `Terelle_Homework_3`. It passed its status checks, and I recorded the
public IPv4 address `3.145.138.109`. I connected through **EC2 Instance Connect**,
and the SEIR dashboard displayed as expected with the same resource profile as the
previous two instances. The prompt `ec2-user@ip-172-31-16-229` confirmed the
connection, captured in `screenshots/deployment-3.png`. I terminated the instance
when I was finished.

---

## 4. What the dashboard tells me about the instance

The dashboard reports several things about the server I am logged in to:

- **CPU** — the instance has 2 vCPUs, and the reading showed the CPU was
  approximately 0% busy at the time of each screenshot, which is expected for a
  freshly booted machine with nothing running on it. The **load average** (around
  0.08–0.43 across the three deployments) is effectively idle: a healthy idle Linux
  system sits near zero, and the lab notes that a load average roughly equal to the
  number of vCPUs means the CPUs are fully busy.
- **Memory** — around 184 MiB used out of roughly 913 MiB total, or about 21% full.
  Most of that is the operating system and the dashboard itself, which tells me the
  instance has plenty of headroom for the exercises in Lab 1.
- **Root disk** — 1.7G used out of 8.0G total, again about 21% full. This is the
  base Amazon Linux 2023 install plus the User Data script and its output, so there
  is ample free space remaining.
- **Uptime** — between 2 and 3 minutes, which confirms the instance had only just
  booted and that I was connecting very soon after launch. It also tells me the
  User Data script had already run to completion, since the dashboard it installs
  was present on login.

Taken together, these readings describe a small, freshly booted, essentially idle
server — exactly what a `t3.micro` running a simple Bash script should look like.

---

## 5. What became easier with repetition

The launch itself got noticeably faster by the third attempt. The first time
through, I was reading each screen carefully to make sure I selected the right AMI,
instance type, key pair, and security group. By the third deployment I recognized
the flow and moved through it quickly, because the launch template already carried
the AMI, instance type, and User Data script — I only had to launch from it rather
than configure those settings again.

A specific lesson from repeating the process was remembering to **add a name to the
instance** in the launch wizard. I missed that on the first deployment, so instance
1 shows `(name not set)` in my table above while deployments 2 and 3 are named
`Terelle_Homework_2` and `Terelle_Homework_3`. I made a point of catching that field
on the later launches so the instances were easy to identify in the console before I
terminated them.

---

## 6. Errors encountered and how I resolved them

I did not hit any hard failures during the three deployments — all three instances
launched, passed their status checks, and accepted my connections.

Two things did catch my attention along the way, and I resolved both:

1. **The Name field in the launch wizard.** I skipped it on the first deployment,
   which left instance 1 unnamed in the console. I resolved this by specifically
   looking for the Name field on the second and third deployments and setting
   `Terelle_Homework_2` and `Terelle_Homework_3` respectively. I did not relaunch
   the first instance to add a name, since the lab does not require named instances
   and doing so would have incurred extra cost for no grading benefit.

2. **Security group configuration.** Recall from the first class session was not
   automatic for me, so I went back to a screenshot I had taken during that session
   to confirm I was configuring the security group correctly. Specifically, I wanted
   to be sure I only modified the **inbound** rules — permitting SSH (TCP 22) from a
   trusted source — and **left the outbound rules unchanged** at their defaults.
   Checking against my class screenshot confirmed I had it right, and the same
   security group configuration worked for all three deployments.

---

## 7. Cleanup and retained resources

- **Launch template:** retained, as required. I preserved the template for the
  homework and for the labs that follow, so I can launch instances from it again
  without reconfiguring AMI, instance type, key pair, security group, or User Data.
- **EC2 instances:** all three practice instances were **terminated** after their
  evidence was captured and written up. No instances were left running.
- **Elastic IP addresses:** none were allocated or purchased, per the lab's
  instructions.

---

## 8. Definition of done

- [x] I completed the instructor-led EC2 deployment.
- [x] I connected to EC2 using the AWS Connect instructions.
- [x] I observed the SEIR dashboard and shared a screenshot in Zoom during class.
- [x] I preserved my launch template.
- [x] I completed three separate EC2 deployments for homework.
- [x] I successfully connected to each of the three instances.
- [x] I recorded **three distinct public IPv4 addresses**.
- [x] I uploaded three screenshots and explanations to my GitHub repository.
- [ ] I submitted my repository link by Tuesday, October 20, 2026, noon Central Time. *(submitted by me through the instructor's designated channel — this repository is the homework evidence)*
- [x] I terminated unused EC2 instances and checked for leftover billable resources.
