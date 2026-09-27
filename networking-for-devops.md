# DevOps-এর জন্য Networking — শূন্য থেকে শুরু

> একজন beginner DevOps learner-এর জন্য সম্পূর্ণ নোট। পড়ে বোঝার জন্য না — **করে শেখার জন্য।**
> প্রতিটা অধ্যায়ে তিনটা জিনিস আছে: ব্যাখ্যা → হাতে-কলমে সমস্যা → ইন্টারভিউ প্রশ্ন।

---

## এই নোট কীভাবে ব্যবহার করবে

১. প্রথমে **[০. শুরুর আগে](#s0)** পড়ো — পুরো নোটের মানচিত্র ওখানে।
২. এরপর সূচিপত্র থেকে যেকোনো টপিকে ঝাঁপ দাও। প্রতিটা অধ্যায়ের শেষে **↑ সূচিপত্রে ফিরে যাও** লিংক আছে।
৩. ব্যাখ্যা পড়ে পরের অংশে যেও না। **হাতে-কলমে** অংশটা আগে করো। না করলে পরদিন ভুলে যাবে।
৪. ইন্টারভিউ প্রশ্নের উত্তর লুকানো আছে (▸ চিহ্নে ক্লিক)। **নিজে মুখে উত্তর দেওয়ার চেষ্টা করার আগে খুলবে না।**

### গভীরতার লেবেল

প্রতিটা টপিকের পাশে লেখা আছে কতটুকু বোঝা লাগবে:

| লেবেল | মানে |
|---|---|
| **[গভীর]** | না বুঝলে কাজ আটকে যাবে। হাতে-কলমে করা বাধ্যতামূলক, মুখস্থে হবে না। |
| **[কাজচালানো]** | কনফিগার করতে পারা আর সমস্যা হলে কোথায় দেখতে হবে জানা — যথেষ্ট। |
| **[নাম-জানা]** | কী জিনিস আর কখন লাগে, এইটুকু। দরকার পড়লে তখন ডকুমেন্টেশন পড়বে। |

শুরুতে শুধু **[গভীর]**-গুলো ধরো। ওগুলো শেষ হলে **[কাজচালানো]**। **[নাম-জানা]** কখনো মুখস্থ কোরো না — শুধু জেনে রাখো জিনিসটা আছে।

---

<a id="index"></a>

## 📑 সূচিপত্র

**[০. শুরুর আগে — মানচিত্র ও ল্যাব সেটআপ](#s0)**

**[১. Fundamentals](#s1)**
- [১.১ IP address](#s1-1) — **[গভীর]**
- [১.২ Subnet ও CIDR](#s1-2) — **[গভীর]**
- [১.৩ OSI ও TCP/IP model](#s1-3) — **[কাজচালানো]**
- [১.৪ TCP, UDP ও connection state](#s1-4) — **[গভীর]**
- [১.৫ Port](#s1-5) — **[গভীর]**
- [১.৬ MTU](#s1-6) — **[কাজচালানো]**

**[২. DNS](#s2)**
- [২.১ নাম থেকে IP — পুরো যাত্রা](#s2-1) — **[গভীর]**
- [২.২ Record type](#s2-2) — **[গভীর]**
- [২.৩ TTL ও caching](#s2-3) — **[গভীর]**
- [২.৪ Linux-এ DNS কোথায় বসে](#s2-4) — **[গভীর]**
- [২.৫ dig দিয়ে কাজ](#s2-5) — **[গভীর]**

**[৩. HTTP ও TLS](#s3)**
- [৩.১ HTTP request-এর শরীর](#s3-1) — **[গভীর]**
- [৩.২ Status code](#s3-2) — **[গভীর]**
- [৩.৩ যে header-গুলো রোজ লাগে](#s3-3) — **[গভীর]**
- [৩.৪ TLS ও certificate](#s3-4) — **[গভীর]**
- [৩.৫ HTTP/2, HTTP/3, WebSocket, CORS](#s3-5) — **[কাজচালানো]**

**[৪. Linux networking](#s4)**
- [৪.১ Interface ও routing table](#s4-1) — **[গভীর]**
- [৪.২ NAT — private IP কীভাবে ইন্টারনেটে যায়](#s4-2) — **[গভীর]**
- [৪.৩ iptables সংক্ষেপে](#s4-3) — **[কাজচালানো]**
- [৪.৪ Network namespace ও veth](#s4-4) — **[কাজচালানো]**

**[৫. Troubleshooting — আসল কাজ](#s5)**
- [৫.১ চার ধাপের runbook](#s5-1) — **[গভীর]**
- [৫.২ টুলবক্স](#s5-2) — **[গভীর]**
- [৫.৩ tcpdump](#s5-3) — **[কাজচালানো]**

**[৬. Cloud networking (AWS)](#s6)**
- [৬.১ VPC-র শরীর](#s6-1) — **[গভীর]**
- [৬.২ Public vs private subnet](#s6-2) — **[গভীর]**
- [৬.৩ Security Group vs NACL](#s6-3) — **[গভীর]**
- [৬.৪ Load balancer — ALB vs NLB](#s6-4) — **[গভীর]**
- [৬.৫ VPC Flow Logs](#s6-5) — **[গভীর]**
- [৬.৬ Peering, Endpoint, VPN, TGW](#s6-6) — **[কাজচালানো]**

**[৭. Container ও Kubernetes networking](#s7)**
- [৭.১ Docker networking](#s7-1) — **[গভীর]**
- [৭.২ Kubernetes-এর নেটওয়ার্ক মডেল](#s7-2) — **[গভীর]**
- [৭.৩ Service-এর ধরন](#s7-3) — **[গভীর]**
- [৭.৪ CoreDNS](#s7-4) — **[গভীর]**
- [৭.৫ Ingress](#s7-5) — **[গভীর]**
- [৭.৬ kube-proxy ও NetworkPolicy](#s7-6) — **[কাজচালানো]**
- [৭.৭ k8s নেটওয়ার্ক ডিবাগ করার নিয়ম](#s7-7) — **[গভীর]**

**[৮. Security](#s8)** — **[গভীর]**

**[৯. আরও উপরে — শুধু নাম জেনে রাখো](#s9)** — **[নাম-জানা]**

**[১০. ৩০ দিনের প্ল্যান](#s10)**

**[১১. চিট শিট](#s11)**

**[১২. দ্রুত ইন্টারভিউ প্রশ্ন — ৩০টা](#s12)**

---

<a id="s0"></a>

## ০. শুরুর আগে — মানচিত্র ও ল্যাব সেটআপ

### পুরো networking একটা বাক্যে

> তুমি ব্রাউজারে `example.com` লিখলে — **নাম থেকে ঠিকানা বের হয় (DNS)**, **ঠিকানায় সংযোগ তৈরি হয় (TCP)**, **সংযোগটা গোপন করা হয় (TLS)**, তারপর **কথাবার্তা হয় (HTTP)**।

এই চারটা ধাপই এই নোটের মেরুদণ্ড। DevOps-এ networking-এর যত সমস্যা আসবে, প্রায় সবগুলো এই চারটার কোনো একটায় আটকে থাকবে। তাই তোমার কাজ দুইটা:

1. চারটা ধাপ আলাদা আলাদা করে বোঝা
2. সমস্যা হলে **কোন ধাপে আটকেছে** সেটা দুই মিনিটে বের করতে পারা

দ্বিতীয়টা প্রথমটার চেয়েও দামি। ইন্টারভিউয়ার সংজ্ঞা শোনার জন্য বসে থাকে না, শোনে তুমি সমস্যায় পড়লে কীভাবে এগোও।

### কেন DevOps-এ networking লাগে

তোমার রোজকার কাজের মধ্যে এগুলো পড়বেই:

- অ্যাপ deploy হলো, কিন্তু বাইরে থেকে খোলা যাচ্ছে না
- দুইটা service একে অপরকে খুঁজে পাচ্ছে না
- HTTPS হঠাৎ কাজ বন্ধ করে দিয়েছে
- staging-এ চলে, production-এ চলে না
- "সাইট স্লো" — কেউ জানে না কেন

এর একটাও application code-এর সমস্যা না। সবগুলোই networking। তাই এই জিনিসটা DevOps-এ optional না।

### ল্যাব সেটআপ (আজকেই করো)

এই নোট পড়তে গেলে হাতের কাছে এগুলো লাগবে:

| জিনিস | কেন | কীভাবে |
|---|---|---|
| একটা Linux টার্মিনাল | সব কমান্ড এখানে চালাবে | WSL2 (Windows), অথবা Multipass/VirtualBox VM |
| Docker | container networking শিখতে | Docker Desktop বা `apt install docker.io` |
| kind বা minikube | k8s networking শিখতে | `kind create cluster` |
| AWS free tier account | VPC হাতে বানাতে | aws.amazon.com/free |
| নেটওয়ার্ক টুল | ডিবাগ করতে | `sudo apt install -y dnsutils iproute2 curl tcpdump mtr netcat-openbsd` |

> **টিপ:** AWS account বানানোর সাথে সাথে একটা billing alarm বসাও ($5)। NAT Gateway ঘণ্টায় টাকা কাটে, ভুলে ফেলে রাখলে মাস শেষে চমক পাবে।

### একটা নিয়ম, যেটা সবচেয়ে বেশি কাজে দেবে

**ইচ্ছা করে ভাঙো, তারপর ঠিক করো।**

পড়ে শেখা আর ভেঙে ঠিক করে শেখা এক জিনিস না। এই নোটের প্রতিটা "হাতে-কলমে" অংশে তোমাকে কিছু একটা ভাঙতে বলা হবে — firewall বন্ধ করা, ভুল DNS দেওয়া, ভুল পোর্টে bind করা। ওই ভাঙা জিনিস ঠিক করার স্মৃতিটাই ইন্টারভিউতে তোমার উত্তর হবে।

[↑ সূচিপত্রে ফিরে যাও](#index)

---

<a id="s1"></a>

## ১. Fundamentals

<a id="s1-1"></a>

### ১.১ IP address — **[গভীর]**

IP address হলো নেটওয়ার্কে একটা মেশিনের **ঠিকানা**। IPv4 ঠিকানা ৩২ বিটের, চারটা অংশে ভাগ করে লেখা হয়:

```
192.168.1.10
 │   │  │ │
 └───┴──┴─┴── চারটা octet, প্রতিটা 0–255
```

#### Private vs Public

দুনিয়ায় IPv4 ঠিকানার সংখ্যা সীমিত (প্রায় ৪২০ কোটি), তাই কিছু রেঞ্জ "ব্যক্তিগত" হিসেবে সরিয়ে রাখা হয়েছে। এগুলো ইন্টারনেটে রাউট হয় না — শুধু তোমার নিজের নেটওয়ার্কের ভেতরে চলে।

| রেঞ্জ | CIDR | সাধারণত কোথায় দেখবে |
|---|---|---|
| 10.0.0.0 – 10.255.255.255 | `10.0.0.0/8` | AWS VPC, বড় corporate নেটওয়ার্ক |
| 172.16.0.0 – 172.31.255.255 | `172.16.0.0/12` | Docker-এর default bridge (`172.17.0.0/16`) |
| 192.168.0.0 – 192.168.255.255 | `192.168.0.0/16` | বাসার রাউটার, ছোট অফিস |

Public IP হলো বাকি সব — যেগুলো ইন্টারনেটে সরাসরি পৌঁছানো যায় এবং যেগুলোর জন্য টাকা লাগে।

#### তিনটা বিশেষ ঠিকানা, যেগুলো না জানলে ভুগবে

| ঠিকানা | মানে |
|---|---|
| `127.0.0.1` (localhost) | "আমি নিজে"। এখানে bind করলে **শুধু একই মেশিন থেকে** পাওয়া যাবে |
| `0.0.0.0` | "সব ইন্টারফেস"। সার্ভার এখানে bind করলে বাইরে থেকেও পাওয়া যাবে |
| `169.254.169.254` | cloud metadata service। AWS-এ instance-এর নিজের তথ্য এখান থেকে আসে |

> **সবচেয়ে কমন beginner ভুল:** container-এর ভেতরে অ্যাপ `127.0.0.1:8080`-এ bind করা হয়েছে, তারপর `docker run -p 8080:8080` দিয়ে বাইরে থেকে খুলতে গিয়ে কাজ করছে না। কারণ container-এর ভেতরের localhost আর বাইরের localhost এক জিনিস না। সমাধান: অ্যাপকে `0.0.0.0`-তে bind করাও।

#### 🔧 হাতে-কলমে

```bash
# ১. তোমার মেশিনের IP কী?
ip addr show

# ২. সেটা private না public? উপরের টেবিলের সাথে মিলাও।

# ৩. তোমার public IP কী? (NAT-এর বাইরে দুনিয়া তোমাকে কী নামে চেনে)
curl -s ifconfig.me; echo

# ৪. দুইটা কি আলাদা? আলাদা হলে — কেন? (উত্তর ৪.২-এ আছে)
```

**ছোট সমস্যা:** একটা Python সার্ভার চালাও দুইভাবে, পার্থক্য দেখো।

```bash
# প্রথমে localhost-এ
python3 -m http.server 8000 --bind 127.0.0.1
# আরেক টার্মিনাল থেকে:  curl http://127.0.0.1:8000    → কাজ করবে
# অন্য মেশিন/container থেকে তোমার IP দিয়ে            → কাজ করবে না

# এবার সব ইন্টারফেসে
python3 -m http.server 8000 --bind 0.0.0.0
# এখন দুইটাই কাজ করবে
```

#### 🎤 ইন্টারভিউ প্রশ্ন

<details>
<summary>▸ <code>0.0.0.0</code> আর <code>127.0.0.1</code>-এ bind করার পার্থক্য কী?</summary>

`127.0.0.1` loopback ইন্টারফেস — শুধু ওই মেশিনের ভেতর থেকে পৌঁছানো যায়। `0.0.0.0` মানে মেশিনের সব নেটওয়ার্ক ইন্টারফেস — বাইরে থেকেও পৌঁছানো যায়।

বাস্তবে এটা container-এ সবচেয়ে বেশি কামড়ায়: container-এর ভেতরে অ্যাপ localhost-এ bind করলে `docker -p` port mapping কাজ করে না, কারণ mapping container-এর নেটওয়ার্ক ইন্টারফেসে ট্রাফিক পাঠায়, loopback-এ না।

নিরাপত্তার দিক থেকে উল্টোটাও সত্য — যে service-টা শুধু লোকালি লাগবে (যেমন একটা admin port), সেটা ইচ্ছা করেই `127.0.0.1`-এ রাখা উচিত।

</details>

<details>
<summary>▸ Private IP কেন দরকার হলো?</summary>

IPv4-তে ঠিকানার সংখ্যা সীমিত। সব ডিভাইসকে public IP দিলে অনেক আগেই ফুরিয়ে যেত। তাই RFC 1918 কিছু রেঞ্জ সংরক্ষিত রাখল, যেগুলো যেকোনো প্রতিষ্ঠান নিজের ভেতরে পুনর্ব্যবহার করতে পারে। বাইরে যাওয়ার সময় NAT সেগুলোকে একটা public IP-তে অনুবাদ করে দেয়।

পাশাপাশি এটা নিরাপত্তাও দেয় — private IP-তে ইন্টারনেট থেকে কেউ সরাসরি সংযোগ করতে পারে না।

</details>

[↑ সূচিপত্রে ফিরে যাও](#index)

---

<a id="s1-2"></a>

### ১.২ Subnet ও CIDR — **[গভীর]**

এটাই networking-এর সবচেয়ে বেশি রিটার্ন দেওয়া টপিক। VPC design, k8s pod CIDR, VPN peering, firewall rule — সবখানে লাগবে।

#### CIDR notation পড়া

```
10.0.1.0/24
         └── prefix: প্রথম ২৪টা বিট "নেটওয়ার্ক", বাকি ৮টা "host"
```

মোট ৩২ বিট। prefix যত বড়, নেটওয়ার্ক তত ছোট।

**সূত্র:** মোট ঠিকানা = 2^(32 − prefix)

| CIDR | মোট IP | AWS-এ ব্যবহারযোগ্য | সাধারণত কীসের জন্য |
|---|---|---|---|
| `/16` | 65,536 | 65,531 | পুরো একটা VPC |
| `/20` | 4,096 | 4,091 | বড় subnet |
| `/24` | 256 | 251 | সাধারণ subnet |
| `/26` | 64 | 59 | ছোট subnet |
| `/28` | 16 | 11 | AWS-এ সবচেয়ে ছোট অনুমোদিত |
| `/32` | 1 | — | একটামাত্র নির্দিষ্ট IP (firewall rule-এ) |

> **কেন AWS-এ ৫টা কম?** প্রতিটা subnet-এ AWS ৫টা ঠিকানা নিজে রাখে: network address, VPC router, DNS, ভবিষ্যতের জন্য সংরক্ষিত, আর broadcast address। সাধারণ নেটওয়ার্কে ২টা কম হয় (network + broadcast) — এই পার্থক্যটা ইন্টারভিউতে জিজ্ঞেস করে।

#### Overlap — যে ভুলটা পরে আর ঠিক করা যায় না

দুইটা নেটওয়ার্কের ঠিকানার রেঞ্জ একটু হলেও মিলে গেলে তাদের মধ্যে রাউটিং সম্ভব না।

```
VPC A: 10.0.0.0/16   →  10.0.0.0  থেকে  10.0.255.255
VPC B: 10.0.5.0/24   →  10.0.5.0  থেকে  10.0.5.255    ❌ A-এর ভেতরেই পড়ছে
VPC C: 10.1.0.0/16   →  10.1.0.0  থেকে  10.1.255.255  ✅ সমস্যা নেই
```

VPC-র CIDR বানানোর পরে আর বদলানো যায় না। তাই আগেই একটা পরিকল্পনা লিখে রাখো:

```
10.0.0.0/16   →  prod
10.1.0.0/16   →  staging
10.2.0.0/16   →  dev
172.20.0.0/16 →  k8s pod network   (VPC-র সাথে overlap নেই — খেয়াল করো)
```

#### 🔧 হাতে-কলমে

```bash
sudo apt install -y ipcalc
ipcalc 10.0.1.0/24
ipcalc 10.0.0.0/16
```

**ছোট সমস্যা ১:** `10.0.0.0/16`-কে চারটা সমান ভাগে ভাগ করো।

<details>
<summary>▸ উত্তর</summary>

চার ভাগ মানে ২ বিট বেশি লাগবে → `/18`। প্রতিটায় 16,384টা ঠিকানা।

```
10.0.0.0/18     →  10.0.0.0   – 10.0.63.255
10.0.64.0/18    →  10.0.64.0  – 10.0.127.255
10.0.128.0/18   →  10.0.128.0 – 10.0.191.255
10.0.192.0/18   →  10.0.192.0 – 10.0.255.255
```

</details>

**ছোট সমস্যা ২:** একটা VPC `10.0.0.0/16`। দুইটা AZ-এ একটা করে public আর একটা করে private subnet লাগবে — মোট ৪টা। প্রতিটায় অন্তত ২০০টা IP চাই। CIDR গুলো লেখো।

<details>
<summary>▸ উত্তর</summary>

২০০টা IP-র জন্য `/24` (২৫১টা ব্যবহারযোগ্য) যথেষ্ট।

```
10.0.1.0/24  →  public-1a
10.0.2.0/24  →  public-1b
10.0.11.0/24 →  private-1a
10.0.12.0/24 →  private-1b
```

খেয়াল করো — public-গুলো 1–9, private-গুলো 11–19 রাখা হয়েছে। এই ধরনের ফাঁক রাখলে পরে subnet যোগ করা সহজ হয়, আর নাম দেখেই ধরন বোঝা যায়।

</details>

**ছোট সমস্যা ৩:** `10.0.1.0/24` আর `10.0.0.0/16` — এই দুইটা কি overlap করে?

<details>
<summary>▸ উত্তর</summary>

হ্যাঁ। `10.0.0.0/16` মানে `10.0.0.0` থেকে `10.0.255.255` পর্যন্ত সব — `10.0.1.x` সেটার ভেতরেই পড়ে। এই দুইটা VPC peer করা যাবে না।

</details>

#### 🎤 ইন্টারভিউ প্রশ্ন

<details>
<summary>▸ <code>/26</code> subnet-এ AWS-এ কয়টা IP ব্যবহার করা যায়?</summary>

2^(32−26) = 64টা মোট। AWS প্রতি subnet-এ ৫টা সংরক্ষণ করে, তাই **৫৯টা** ব্যবহারযোগ্য।

সংরক্ষিত ৫টা: `.0` (network), `.1` (VPC router), `.2` (DNS), `.3` (ভবিষ্যতের জন্য), আর শেষেরটা (broadcast)।

</details>

<details>
<summary>▸ VPC-র CIDR কীভাবে ঠিক করবে?</summary>

তিনটা জিনিস মাথায় রেখে:

১. **বড় করে নাও** — `/16` নিলে ক্ষতি নেই, private IP ফ্রি। ছোট নিলে পরে বাড়াতে কষ্ট।
২. **অন্য কোনো নেটওয়ার্কের সাথে overlap করা যাবে না** — অন্য VPC, অফিসের নেটওয়ার্ক, VPN-এর ওপারের নেটওয়ার্ক, এমনকি k8s-এর pod CIDR।
৩. **আগে থেকে একটা বণ্টন তালিকা লিখে রাখো** — কোন environment কোন রেঞ্জ পাবে।

সবচেয়ে বড় ফাঁদ: সবাই default-এ `10.0.0.0/16` নেয়। পরে দুইটা account peer করতে গেলে দুইটারই একই CIDR — তখন আর কিছু করার থাকে না, নতুন VPC বানিয়ে migrate করতে হয়।

</details>

[↑ সূচিপত্রে ফিরে যাও](#index)

---

<a id="s1-3"></a>

### ১.৩ OSI ও TCP/IP model — **[কাজচালানো]**

সাতটা লেয়ার মুখস্থ করার দরকার নেই। DevOps-এ বাস্তবে তিনটা লেয়ার লাগে:

| লেয়ার | কী নিয়ে কাজ | তুমি কোথায় দেখবে |
|---|---|---|
| **L3 — Network** | IP address, routing | VPC route table, `ip route`, subnet |
| **L4 — Transport** | TCP/UDP, port | NLB, Security Group, `ss` |
| **L7 — Application** | HTTP, DNS, TLS | ALB, Ingress, `curl` |

কেন এটুকু লাগে: **load balancer বাছার সময়।** L4 balancer শুধু IP আর port দেখে ট্রাফিক পাঠায় — দ্রুত, কিন্তু URL বা header দেখতে পারে না। L7 balancer পুরো HTTP request পড়তে পারে — তাই path দেখে আলাদা service-এ পাঠাতে পারে, কিন্তু একটু ধীর।

**মনে রাখার সহজ উপায়:** নিচের লেয়ার যত কম জানে, তত দ্রুত চলে।

[↑ সূচিপত্রে ফিরে যাও](#index)

---

<a id="s1-4"></a>

### ১.৪ TCP, UDP ও connection state — **[গভীর]**

#### TCP বনাম UDP

| | TCP | UDP |
|---|---|---|
| সংযোগ | আগে handshake করে | সরাসরি পাঠায় |
| নির্ভরযোগ্যতা | প্যাকেট হারালে আবার পাঠায় | হারালে হারালো |
| ক্রম | ঠিক ক্রমে পৌঁছায় | ক্রমের নিশ্চয়তা নেই |
| গতি | তুলনামূলক ধীর | দ্রুত |
| কোথায় | HTTP, SSH, database | DNS, ভিডিও স্ট্রিম, গেম, VPN |

#### 3-way handshake

```
Client                      Server
  │  ──── SYN ────────────▶  │   "কথা বলতে চাই"
  │  ◀─── SYN-ACK ────────   │   "চলো, আমিও রাজি"
  │  ──── ACK ────────────▶  │   "ঠিক আছে, শুরু"
  │      সংযোগ ESTABLISHED   │
```

#### Connection state — যেগুলো দেখে সমস্যা ধরা যায়

`ss -tan` চালালে প্রতিটা সংযোগের একটা অবস্থা দেখাবে:

| State | মানে | চিন্তার কারণ? |
|---|---|---|
| `LISTEN` | পোর্ট খোলা আছে, অপেক্ষা করছে | না — স্বাভাবিক |
| `ESTABLISHED` | সক্রিয় সংযোগ | না |
| `TIME_WAIT` | সংযোগ শেষ, ~৬০ সেকেন্ড অপেক্ষা | না — স্বাভাবিক |
| `CLOSE_WAIT` | ওপাশ বন্ধ করেছে, **তোমার অ্যাপ করেনি** | **হ্যাঁ** — অ্যাপ socket close করছে না, বাগ |
| `SYN_SENT` (আটকে) | SYN গেছে, উত্তর আসেনি | **হ্যাঁ** — firewall ব্লক করছে |

> **মনে রাখো:** `TIME_WAIT` জমলে সেটা সাধারণত ঠিক আছে। `CLOSE_WAIT` জমলে সেটা প্রায় সবসময় application-এর বাগ — connection leak।

#### সবচেয়ে দরকারি একটা পার্থক্য

```bash
curl http://10.0.1.5:8080

# → curl: (7) Failed to connect: Connection refused
#   মানে: প্যাকেট পৌঁছেছে, কিন্তু ওই পোর্টে কেউ শুনছে না
#   সন্দেহ করো: service বন্ধ, ভুল পোর্ট, ভুল bind address

# → curl: (28) Connection timed out
#   মানে: প্যাকেট গেছে, কোনো উত্তরই আসেনি
#   সন্দেহ করো: Security Group, NACL, firewall, ভুল রুট
```

এই এক লাইনের পার্থক্যই তোমার ডিবাগের দিক ঠিক করে দেবে। **refused = অ্যাপের সমস্যা। timeout = নেটওয়ার্কের সমস্যা।**

#### 🔧 হাতে-কলমে

```bash
# টার্মিনাল ১ — একটা সার্ভার চালাও
nc -l 9000

# টার্মিনাল ২ — সংযোগ করো, কিছু লেখো
nc 127.0.0.1 9000

# টার্মিনাল ৩ — অবস্থা দেখো
ss -tan | grep 9000
# ESTABLISHED দেখবে দুইটা (দুই দিক থেকে)

# এবার টার্মিনাল ১ বন্ধ করো (Ctrl+C), আবার দেখো
ss -tan | grep 9000
# TIME_WAIT দেখবে
```

**ছোট সমস্যা:** নিজে দুইটা ভিন্ন ত্রুটি তৈরি করো।

```bash
# refused বানাও — যে পোর্টে কেউ নেই
curl -m 5 http://127.0.0.1:9999

# timeout বানাও — এমন IP যেখানে কেউ নেই
curl -m 5 http://10.255.255.1:80

# দুইটার error message লিখে রাখো। এই পার্থক্যটা তোমার আজীবন কাজে লাগবে।
```

#### 🎤 ইন্টারভিউ প্রশ্ন

<details>
<summary>▸ "Connection refused" আর "Connection timeout"-এর পার্থক্য কী? কোনটা দেখলে কোথায় খুঁজবে?</summary>

**Refused:** প্যাকেট গন্তব্যে পৌঁছেছে, কিন্তু ওই পোর্টে কোনো process শুনছে না — তাই OS একটা `RST` প্যাকেট ফেরত পাঠিয়েছে। মানে নেটওয়ার্ক পথ ঠিক আছে। খুঁজবে: service চলছে কিনা (`systemctl status`), ঠিক পোর্টে কিনা (`ss -tulpn`), ঠিক ইন্টারফেসে bind হয়েছে কিনা।

**Timeout:** কোনো উত্তরই আসেনি — প্যাকেট কোথাও নীরবে ফেলে দেওয়া হয়েছে। firewall সাধারণত DROP করে, REJECT করে না, তাই এমন হয়। খুঁজবে: Security Group, NACL, iptables, route table, ভুল IP।

এক লাইনে: **refused = অ্যাপ পর্যন্ত পৌঁছেছি, timeout = পৌঁছাইনি।**

</details>

<details>
<summary>▸ সার্ভারে হাজার হাজার <code>CLOSE_WAIT</code> দেখছো। কী বুঝলে?</summary>

`CLOSE_WAIT` মানে ওপাশ সংযোগ বন্ধ করেছে (FIN পাঠিয়েছে), কিন্তু তোমার পাশের application এখনো socket বন্ধ করেনি। এটা প্রায় সবসময় **application-এর বাগ** — connection বা file descriptor leak, সাধারণত কোনো HTTP client বা connection pool ঠিকমতো close করা হয়নি।

এটা নেটওয়ার্ক সমস্যা না, তাই firewall বা LB ঘেঁটে লাভ নেই। জমতে থাকলে শেষ পর্যন্ত file descriptor limit শেষ হয়ে অ্যাপ নতুন সংযোগ নিতে পারবে না।

তুলনায় `TIME_WAIT` জমা স্বাভাবিক — ওটা TCP-র নিজের নিয়ম, সংযোগ বন্ধের পর ~৬০ সেকেন্ড অপেক্ষা।

</details>

<details>
<summary>▸ DNS কেন UDP ব্যবহার করে?</summary>

DNS query ছোট আর উত্তরও ছোট — একটা প্যাকেটেই হয়ে যায়। TCP handshake করতে তিনটা প্যাকেট লাগত, যা পুরো query-র চেয়েও বেশি খরচ। হারিয়ে গেলে আবার জিজ্ঞেস করলেই হলো।

তবে উত্তর বড় হলে (৫১২ বাইটের বেশি, যেমন DNSSEC বা অনেক record) DNS **TCP-তেও** চলে যায়। তাই firewall-এ DNS-এর জন্য 53 পোর্ট UDP আর TCP দুইটাতেই খুলতে হয় — এটা একটা ক্লাসিক ভুল।

</details>

[↑ সূচিপত্রে ফিরে যাও](#index)

---

<a id="s1-5"></a>

### ১.৫ Port — **[গভীর]**

IP বলে **কোন মেশিন**, port বলে **ওই মেশিনের কোন service**। ০ থেকে ৬৫৫৩৫ পর্যন্ত।

#### মুখস্থ করে ফেলো

| পোর্ট | Service | মন্তব্য |
|---|---|---|
| 22 | SSH | `0.0.0.0/0`-তে খোলা রাখা বিপজ্জনক |
| 53 | DNS | UDP **এবং** TCP দুইটাই |
| 80 | HTTP | |
| 443 | HTTPS | |
| 3306 | MySQL | কখনো ইন্টারনেটে খুলবে না |
| 5432 | PostgreSQL | কখনো ইন্টারনেটে খুলবে না |
| 6379 | Redis | default-এ পাসওয়ার্ড নেই — খুব সাবধান |
| 6443 | Kubernetes API server | |
| 27017 | MongoDB | |
| 9090 / 3000 | Prometheus / Grafana | |

**Ephemeral port (32768–60999):** client যখন সংযোগ করে, তখন নিজের জন্য একটা এলোমেলো পোর্ট নেয়। তাই `ss`-এ অদ্ভুত বড় নম্বর দেখলে ঘাবড়ানোর কিছু নেই।

#### 🔧 হাতে-কলমে

```bash
# তোমার মেশিনে কোন কোন পোর্ট খোলা, আর কোন process সেটা ধরে আছে?
sudo ss -tulpn
#  -t TCP  -u UDP  -l শুধু LISTEN  -p process  -n নাম না, নম্বর দেখাও

# একটা নির্দিষ্ট পোর্টে কে বসে আছে?
sudo ss -tulpn | grep :80

# দূরের মেশিনের একটা পোর্ট খোলা কিনা?
nc -zv google.com 443
nc -zv google.com 444    # এটা ঝুলে থাকবে → firewall drop করছে
```

**ছোট সমস্যা:** "Address already in use" ত্রুটি তৈরি করো আর ঠিক করো।

```bash
python3 -m http.server 8000 &     # প্রথমটা চলবে
python3 -m http.server 8000       # দ্বিতীয়টা ত্রুটি দেবে

# কে পোর্টটা ধরে আছে বের করো, তারপর মারো
sudo ss -tulpn | grep :8000
kill %1
```

#### 🎤 ইন্টারভিউ প্রশ্ন

<details>
<summary>▸ একটা সার্ভারে কোন পোর্টে কী চলছে কীভাবে দেখবে?</summary>

```bash
sudo ss -tulpn
```

`-t` TCP, `-u` UDP, `-l` শুধু listening socket, `-p` কোন process, `-n` DNS lookup না করে সরাসরি নম্বর (দ্রুত হয়)।

পুরোনো সিস্টেমে `netstat -tulpn`, কিন্তু `ss` দ্রুত আর এখন এটাই স্ট্যান্ডার্ড। আউটপুটে `Local Address` কলামে `0.0.0.0:80` আর `127.0.0.1:80`-এর পার্থক্য খেয়াল করা জরুরি — দ্বিতীয়টা বাইরে থেকে পাওয়া যাবে না।

</details>

[↑ সূচিপত্রে ফিরে যাও](#index)

---

<a id="s1-6"></a>

### ১.৬ MTU — **[কাজচালানো]**

MTU = এক প্যাকেটে সর্বোচ্চ কত বাইট পাঠানো যাবে। সাধারণ ইথারনেটে **১৫০০**।

কেন জানা দরকার: VPN বা k8s overlay network প্রতিটা প্যাকেটে বাড়তি header যোগ করে, তাই আসল data-র জায়গা কমে যায়। MTU ঠিকমতো সেট না থাকলে বড় প্যাকেট নীরবে হারায়।

**লক্ষণ চিনে রাখো:** ছোট request কাজ করে, বড় request ঝুলে যায়। `ping` কাজ করে কিন্তু `curl` আটকে থাকে। SSH connect হয় কিন্তু `ls` চালালে জমে যায়।

এই লক্ষণ দেখলেই MTU সন্দেহ করো:

```bash
# fragment না করে ১৪৭২ বাইট পাঠাও (১৫০০ − ২৮ বাইট header)
ping -M do -s 1472 8.8.8.8      # কাজ করলে MTU 1500 ঠিক আছে
ping -M do -s 1400 8.8.8.8      # ছোটটা কাজ করে বড়টা না করলে MTU কম
```

[↑ সূচিপত্রে ফিরে যাও](#index)

---

<a id="s2"></a>

## ২. DNS

DNS ছাড়া ইন্টারনেট চলে না, আর DevOps-এ রহস্যময় সমস্যার সবচেয়ে বড় অংশ এখান থেকেই আসে। একটা পুরোনো কথা আছে — *"It's always DNS."* কথাটা মজার, কিন্তু বেশিরভাগ সময় সত্যি।

<a id="s2-1"></a>

### ২.১ নাম থেকে IP — পুরো যাত্রা — **[গভীর]**

তুমি `blog.example.com` লিখলে কী ঘটে:

```
১. ব্রাউজারের cache          → আগে জিজ্ঞেস করেছি? থাকলে শেষ।
২. OS-এর cache + /etc/hosts  → এখানে লেখা থাকলে DNS-এ যাবেই না
৩. Resolver (ISP / 8.8.8.8)  → এর cache-এ আছে?
        │ না থাকলে ↓
৪. Root server (.)           → "com-এর দায়িত্বে কে? — এই TLD server"
৫. TLD server (.com)         → "example.com-এর দায়িত্বে কে? — এই nameserver"
৬. Authoritative NS          → "blog.example.com = 93.184.216.34"
৭. উত্তর ফিরে আসে, resolver TTL অনুযায়ী cache করে রাখে
```

দুইটা জিনিস এখানে মনে রাখা জরুরি:

- **`/etc/hosts` সবার আগে।** ওখানে ভুল কিছু লেখা থাকলে DNS যত ঠিকই হোক, কাজ হবে না। ডিবাগের সময় এটা সবার আগে দেখো।
- **Cache মাঝখানে বসে আছে।** তুমি DNS record বদলালেও পুরোনো উত্তর TTL শেষ না হওয়া পর্যন্ত টিকে থাকবে।

<a id="s2-2"></a>

### ২.২ Record type — **[গভীর]**

| Type | কাজ | উদাহরণ |
|---|---|---|
| **A** | নাম → IPv4 | `example.com → 93.184.216.34` |
| **AAAA** | নাম → IPv6 | `example.com → 2606:2800:220:1::` |
| **CNAME** | নাম → আরেকটা নাম | `www.example.com → example.com` |
| **MX** | মেইল সার্ভার | `example.com → mail.example.com (priority 10)` |
| **TXT** | যেকোনো লেখা | SPF, DKIM, domain ownership যাচাই |
| **NS** | এই domain-এর দায়িত্বে কোন nameserver | `example.com → ns1.provider.com` |
| **SRV** | service + port | k8s-এ headless service-এ লাগে |
| **PTR** | IP → নাম (reverse) | মেইল সার্ভারের জন্য জরুরি |

#### CNAME-এর দুইটা নিয়ম, যেগুলো ইন্টারভিউতে আসে

১. **root domain-এ CNAME দেওয়া যায় না।** `example.com`-এ CNAME বসালে তার পাশে MX বা NS record থাকতে পারে না — DNS স্ট্যান্ডার্ড এটা নিষেধ করে। তাই cloud provider-রা নিজস্ব সমাধান দেয়: Route 53-এ **ALIAS** record, অন্য জায়গায় ANAME/flattened CNAME।

২. **CNAME চেইন খরচ বাড়ায়।** `a → b → c → IP` মানে তিন দফা lookup। দুই-তিনটার বেশি চেইন হলে latency বাড়বে।

<a id="s2-3"></a>

### ২.৩ TTL ও caching — **[গভীর]**

TTL (Time To Live) = এই উত্তরটা কত সেকেন্ড cache করে রাখা যাবে।

```
example.com.    300    IN    A    93.184.216.34
                └── TTL: ৩০০ সেকেন্ড = ৫ মিনিট
```

**কেন এটা তোমার জীবনে গুরুত্বপূর্ণ:** সার্ভার migrate করার দিন যদি TTL ৮৬৪০০ (এক দিন) থাকে, তাহলে DNS বদলানোর পরেও পুরো একদিন ধরে কিছু ব্যবহারকারী পুরোনো সার্ভারে যেতে থাকবে।

**সঠিক নিয়ম:**

```
migration-এর ২ দিন আগে   →  TTL ৮৬৪০০ থেকে ৩০০-তে নামাও
                             (পুরোনো TTL শেষ হওয়ার জন্য সময় দাও)
migration-এর দিন          →  record বদলাও, ৫ মিনিটে সবাই নতুন IP পাবে
migration-এর ২ দিন পরে    →  TTL আবার বাড়িয়ে দাও
```

<a id="s2-4"></a>

### ২.৪ Linux-এ DNS কোথায় বসে — **[গভীর]**

| ফাইল | কাজ |
|---|---|
| `/etc/hosts` | হাতে লেখা ম্যাপিং। **সবার আগে দেখা হয়** |
| `/etc/resolv.conf` | কোন DNS সার্ভার ব্যবহার হবে (`nameserver`), আর search domain |
| `/etc/nsswitch.conf` | কোন ক্রমে খোঁজা হবে (`hosts: files dns`) |

```bash
cat /etc/resolv.conf
# nameserver 10.0.0.2
# search ap-south-1.compute.internal
# options ndots:5
```

**`search` domain কী করে:** তুমি `db` লিখলে সে চেষ্টা করবে `db.ap-south-1.compute.internal`। এজন্যই k8s-এ শুধু `my-service` লিখলেই কাজ করে।

**`ndots:5` কেন জানা দরকার:** k8s-এ এটা default। মানে — নামের মধ্যে ৫টার কম ডট থাকলে আগে সব search domain চেষ্টা করবে, তারপর আসল নামটা। ফলে `api.github.com` (২টা ডট) resolve করতে গিয়ে ৪-৫টা ব্যর্থ query হয় প্রথমে। **k8s ক্লাস্টারে DNS ধীর লাগার এটা সবচেয়ে কমন কারণ।** সমাধান: বাইরের নামের শেষে একটা ডট দাও (`api.github.com.`) অথবা pod-এ `dnsConfig` দিয়ে ndots কমাও।

<a id="s2-5"></a>

### ২.৫ dig দিয়ে কাজ — **[গভীর]**

```bash
# সাধারণ lookup
dig example.com

# শুধু উত্তরটা, ঝামেলা ছাড়া
dig +short example.com

# নির্দিষ্ট record type
dig example.com MX
dig example.com TXT
dig example.com NS

# নির্দিষ্ট DNS সার্ভারকে জিজ্ঞেস করো (cache বাদ দিয়ে যাচাই করতে)
dig @8.8.8.8 example.com
dig @1.1.1.1 example.com

# root থেকে পুরো পথ দেখো — কোথায় আটকাচ্ছে বোঝার সেরা উপায়
dig +trace example.com

# authoritative সার্ভারকে সরাসরি জিজ্ঞেস করো (cache সম্পূর্ণ বাদ)
dig @ns1.example.com example.com
```

**আউটপুট পড়া:**

```
;; ANSWER SECTION:
example.com.        300    IN    A    93.184.216.34
    │                │     │     │         │
    │                │     │     │         └── উত্তর
    │                │     │     └── record type
    │                │     └── class (সবসময় IN)
    │                └── TTL — বারবার চালালে কমতে দেখবে (cache-এ আছে)
    └── নাম
```

> **ছোট টিপ:** `dig` দুইবার চালিয়ে TTL কমতে দেখলে বুঝবে উত্তরটা cache থেকে আসছে। TTL প্রতিবার একই থাকলে বুঝবে authoritative সার্ভার থেকে সরাসরি আসছে।

#### 🔧 হাতে-কলমে

```bash
# ১. তিনটা ভিন্ন resolver-কে একই প্রশ্ন করো, উত্তর মেলে কিনা দেখো
dig +short example.com
dig +short @8.8.8.8 example.com
dig +short @1.1.1.1 example.com

# ২. পুরো পথ দেখো
dig +trace github.com

# ৩. TTL কমতে দেখো
dig example.com | grep -A1 "ANSWER SECTION"
sleep 10
dig example.com | grep -A1 "ANSWER SECTION"

# ৪. একটা CNAME চেইন দেখো
dig www.github.com
```

**ছোট সমস্যা ১:** ইচ্ছা করে DNS ভাঙো।

```bash
# /etc/hosts-এ একটা ভুল ম্যাপিং বসাও
echo "1.2.3.4 example.com" | sudo tee -a /etc/hosts

curl -v -m 5 http://example.com     # এখন কী হয়?
dig +short example.com              # dig কিন্তু ঠিক উত্তরই দেবে — কেন?

# পরিষ্কার করো
sudo sed -i '/1.2.3.4 example.com/d' /etc/hosts
```

<details>
<summary>▸ dig ঠিক উত্তর দিলেও curl কেন ভুল জায়গায় গেল?</summary>

`dig` সরাসরি DNS সার্ভারকে জিজ্ঞেস করে — সে `/etc/hosts` দেখে না। কিন্তু `curl` (আর অন্য সব সাধারণ অ্যাপ) OS-এর resolver ব্যবহার করে, আর সেটা `/etc/nsswitch.conf` অনুযায়ী **আগে `/etc/hosts`** দেখে।

এজন্য একটা জরুরি ডিবাগ নিয়ম: **`dig` ঠিক বলছে মানেই অ্যাপ ঠিক জায়গায় যাচ্ছে না।** দুইটা মিলছে না দেখলে সবার আগে `/etc/hosts` দেখো।

`getent hosts example.com` চালালে অ্যাপ যা দেখে ঠিক তা-ই দেখবে।

</details>

**ছোট সমস্যা ২:** তোমার পছন্দের একটা ওয়েবসাইটের MX, NS আর TXT record বের করো। TXT-তে SPF record খুঁজে বের করতে পারো কিনা দেখো।

#### 🎤 ইন্টারভিউ প্রশ্ন

<details>
<summary>▸ A record আর CNAME-এর পার্থক্য কী? কখন কোনটা ব্যবহার করবে?</summary>

A record নামকে সরাসরি একটা IPv4 ঠিকানায় পাঠায়। CNAME নামকে আরেকটা **নামে** পাঠায়, তারপর সেই নামটা আবার resolve হয়।

CNAME ব্যবহার করবে যখন গন্তব্যের IP বদলাতে পারে আর সেটা অন্য কেউ নিয়ন্ত্রণ করে — যেমন `www.myapp.com → my-alb-123.ap-south-1.elb.amazonaws.com`। ALB-র IP AWS যেকোনো সময় বদলাতে পারে, কিন্তু নামটা একই থাকে।

A record ব্যবহার করবে যখন IP তোমার নিয়ন্ত্রণে আর স্থির — যেমন একটা Elastic IP।

মূল সীমাবদ্ধতা: **root domain-এ (`example.com`) CNAME দেওয়া যায় না**, কারণ তার পাশে MX/NS record থাকে। সমাধান Route 53-এ ALIAS record — দেখতে CNAME-এর মতো কাজ করে কিন্তু A record হিসেবেই ফেরত আসে।

</details>

<details>
<summary>▸ DNS record বদলেছি কিন্তু ট্রাফিক এখনো পুরোনো সার্ভারে যাচ্ছে। কেন, আর কী করবে?</summary>

প্রায় নিশ্চিতভাবে **TTL-এর কারণে cache**। পুরোনো উত্তরটা resolver, OS আর ব্রাউজারে TTL শেষ না হওয়া পর্যন্ত থেকে যাবে।

যাচাই করব এভাবে:
- `dig @8.8.8.8 example.com` আর `dig @ns1.provider.com example.com` — authoritative সার্ভার নতুন উত্তর দিচ্ছে কিন্তু public resolver পুরোনো দিচ্ছে মানেই cache
- `dig example.com` চালিয়ে TTL দেখব — আর কতক্ষণ বাকি

করণীয়: TTL শেষ হওয়া পর্যন্ত অপেক্ষা ছাড়া নির্ভরযোগ্য উপায় নেই। নিজের মেশিনে cache flush করা যায়, কিন্তু ব্যবহারকারীদের করা যায় না।

আর ভুলটা যেন পরেরবার না হয়: **migration-এর অন্তত দুই দিন আগে TTL কমিয়ে ৩০০ করে রাখতে হয়।**

সেই সাথে দেখব `/etc/hosts`-এ পুরোনো entry পড়ে আছে কিনা — এটাও খুব কমন।

</details>

<details>
<summary>▸ Kubernetes-এ DNS lookup ধীর লাগছে। কী দেখবে?</summary>

প্রথমে `ndots` সন্দেহ করব। k8s-এ pod-এর `/etc/resolv.conf`-এ default `options ndots:5` থাকে। মানে ৫টার কম ডটওয়ালা যেকোনো নাম আগে সব search domain-এর সাথে জোড়া লাগিয়ে চেষ্টা করা হবে।

তাই `api.github.com` resolve করতে গিয়ে প্রথমে `api.github.com.default.svc.cluster.local`, তারপর `api.github.com.svc.cluster.local`... — সব ব্যর্থ হওয়ার পরে আসলটা। প্রতিটা external lookup-এ ৪-৫টা বাড়তি query।

সমাধান: external নামের শেষে একটা ডট (`api.github.com.` — FQDN, search বাদ যায়), অথবা pod spec-এ `dnsConfig` দিয়ে `ndots: 2`, অথবা NodeLocal DNSCache বসানো।

এছাড়া দেখব CoreDNS pod-এর CPU/memory limit-এ আটকাচ্ছে কিনা, আর `kubectl logs -n kube-system -l k8s-app=kube-dns`-এ error আছে কিনা।

</details>

[↑ সূচিপত্রে ফিরে যাও](#index)

---

<a id="s3"></a>

## ৩. HTTP ও TLS

<a id="s3-1"></a>

### ৩.১ HTTP request-এর শরীর — **[গভীর]**

```
GET /api/users?page=2 HTTP/1.1          ← method, path, version
Host: api.example.com                    ← কোন সাইট চাইছি
Authorization: Bearer eyJhbGc...         ← header
Accept: application/json
                                         ← ফাঁকা লাইন
{ body — POST/PUT হলে এখানে }
```

উত্তর:

```
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 154

{"users": [...]}
```

**Method:** `GET` (পড়া), `POST` (নতুন বানানো), `PUT` (পুরোটা বদলানো), `PATCH` (অংশ বদলানো), `DELETE` (মোছা), `HEAD` (শুধু header, body বাদ)।

<a id="s3-2"></a>

### ৩.২ Status code — **[গভীর]**

| শ্রেণি | মানে | দায় কার |
|---|---|---|
| **2xx** | সফল | — |
| **3xx** | অন্য জায়গায় যাও | — |
| **4xx** | **client-এর ভুল** | যে request পাঠিয়েছে |
| **5xx** | **server-এর ভুল** | যে চালাচ্ছে, অর্থাৎ তুমি |

এই 4xx/5xx পার্থক্যটাই alert লেখার ভিত্তি। ব্যবহারকারী ভুল URL দিলে (404) তোমাকে রাতে ফোন করা উচিত না, কিন্তু 500 বেড়ে গেলে উচিত।

#### যেগুলো রোজ দেখবে

| কোড | মানে | সাধারণ কারণ |
|---|---|---|
| **301 / 302** | স্থায়ী / অস্থায়ী redirect | HTTP→HTTPS, পুরোনো URL |
| **400** | Bad Request | ভাঙা JSON, ভুল প্যারামিটার |
| **401** | Unauthorized | **তুমি কে জানি না** — টোকেন নেই বা ভুল |
| **403** | Forbidden | **তুমি কে জানি, কিন্তু অনুমতি নেই** |
| **404** | Not Found | ভুল path, অথবা route মেলেনি |
| **429** | Too Many Requests | rate limit |
| **500** | Internal Server Error | অ্যাপে unhandled exception |
| **502** | Bad Gateway | **upstream মরা বা আবোলতাবোল উত্তর দিয়েছে** |
| **503** | Service Unavailable | কোনো healthy backend নেই, বা ইচ্ছাকৃত maintenance |
| **504** | Gateway Timeout | **upstream সময়মতো উত্তর দেয়নি** |

> **৫০২ vs ৫০৪ — মুখস্থ করে ফেলো।** দুইটাই বলে সমস্যা proxy-র পেছনে, কিন্তু আলাদা সমস্যা:
> - **502** = upstream-এর সাথে কথা বলতেই পারলাম না, বা সে ভাঙা উত্তর দিল। → অ্যাপ crash করেছে? পোর্ট ভুল? Service-এর পেছনে কোনো pod নেই?
> - **504** = upstream আছে, কিন্তু নির্দিষ্ট সময়ের মধ্যে উত্তর দিল না। → query ধীর? deadlock? timeout সেটিং কম?

<a id="s3-3"></a>

### ৩.৩ যে header-গুলো রোজ লাগে — **[গভীর]**

| Header | কাজ |
|---|---|
| `Host` | একই IP-তে বহু সাইট চললে কোনটা চাইছি — virtual hosting-এর ভিত্তি |
| `X-Forwarded-For` | proxy-র পেছনে **আসল client-এর IP** |
| `X-Forwarded-Proto` | আসল request HTTP না HTTPS ছিল |
| `Content-Type` | body-র ধরন (`application/json`) |
| `Authorization` | টোকেন বা credential |
| `Cache-Control` | কতক্ষণ cache করা যাবে |
| `User-Agent` | কোন client পাঠিয়েছে |

#### দুইটা বাস্তব সমস্যা যা এই header থেকে হয়

**১. লগে সব IP একই দেখাচ্ছে।** Load balancer-এর পেছনে অ্যাপ বসলে সে প্রতিটা request-এ LB-র IP দেখে। আসল ব্যবহারকারীর IP থাকে `X-Forwarded-For` header-এ। অ্যাপকে ওটা পড়তে বলতে হবে (যেমন Express-এ `trust proxy`, Django-তে `USE_X_FORWARDED_HOST`)।

**২. Redirect loop (ERR_TOO_MANY_REDIRECTS)।** ALB HTTPS-এ TLS শেষ করে HTTP-তে অ্যাপে পাঠায়। অ্যাপ দেখে "এটা তো HTTP!" আর HTTPS-এ redirect করে। LB আবার HTTP-তে পাঠায়। অনন্ত লুপ। সমাধান: অ্যাপকে `X-Forwarded-Proto` header দেখতে বলো, নিজের সংযোগ না।

<a id="s3-4"></a>

### ৩.৪ TLS ও certificate — **[গভীর]**

TLS তিনটা জিনিস দেয়: **গোপনীয়তা** (কেউ পড়তে পারবে না), **অখণ্ডতা** (কেউ বদলাতে পারবে না), আর **পরিচয়** (সার্ভার সত্যিই যে বলছে সে-ই)।

#### Handshake, সংক্ষেপে

```
Client                                Server
  │ ── ClientHello ─────────────────▶ │  "এই cipher-গুলো জানি, সাইট: example.com (SNI)"
  │ ◀── ServerHello + Certificate ─── │  "এই cipher নিলাম, এই আমার সনদ"
  │    সনদ যাচাই করে (CA চেইন, নাম, মেয়াদ)
  │ ── key exchange ────────────────▶ │
  │    ◀── এনক্রিপ্টেড সংযোগ চালু ──▶  │
```

#### Certificate chain

```
তোমার সনদ (example.com)
      ↑ সই করেছে
Intermediate CA
      ↑ সই করেছে
Root CA  ←── এটা ব্রাউজার/OS-এ আগে থেকেই বিশ্বাসযোগ্য হিসেবে বসানো
```

> **সবচেয়ে কমন TLS বাগ:** সার্ভারে শুধু নিজের সনদ বসানো হয়েছে, **intermediate বাদ পড়েছে**। ব্রাউজারে কাজ করে (সে অনেক সময় নিজে খুঁজে নেয়) কিন্তু `curl` বা Java অ্যাপে ব্যর্থ হয়। লক্ষণ: "unable to get local issuer certificate"। সমাধান: fullchain বসাও, শুধু cert না।

#### মুখস্থ কমান্ড

```bash
# সনদ দেখো — কার নামে, কতদিন, কে সই করেছে
echo | openssl s_client -connect example.com:443 -servername example.com 2>/dev/null \
  | openssl x509 -noout -subject -issuer -dates

# পুরো চেইন দেখো (কয় ধাপ আছে?)
openssl s_client -connect example.com:443 -showcerts </dev/null

# curl দিয়ে দ্রুত দেখা
curl -vI https://example.com 2>&1 | grep -E "expire|subject|issuer|SSL"

# মেয়াদ শেষ হওয়া সনদ থাকলেও জোর করে চালাও (শুধু ডিবাগের জন্য)
curl -k https://example.com
```

> `curl -k` কাজ করে কিন্তু `curl` করে না — এর মানে সমস্যাটা **শুধুই certificate-এ**। নেটওয়ার্ক, DNS, অ্যাপ সবই ঠিক আছে। এই একটা পরীক্ষা অনেক সময় বাঁচায়।

#### SNI — এক IP-তে বহু সাইট

TLS handshake শুরু হয় HTTP header পড়ার **আগে**। তাহলে একই IP-তে ১০টা সাইট থাকলে সার্ভার কোন সনদ দেখাবে? এজন্যই SNI — ClientHello-তেই client নামটা বলে দেয়।

পুরোনো client SNI না পাঠালে সার্ভার ভুল সনদ দেয়, আর নামে মিল না হওয়ায় ত্রুটি আসে। `openssl s_client`-এ `-servername` দিতে ভুলে গেলেও ঠিক এই ভুলটাই হয়।

#### 🔧 হাতে-কলমে

```bash
# ১. একটা সাইটের পুরো যাত্রা দেখো — DNS, TCP, TLS, HTTP একসাথে
curl -v https://example.com

# আউটপুটে খুঁজে বের করো:
#   * Trying 93.184.216.34:443        ← DNS resolve হলো
#   * Connected to example.com        ← TCP হলো
#   * TLSv1.3 (OUT), TLS handshake    ← TLS হলো
#   * subject: CN=example.com         ← সনদ
#   > GET / HTTP/1.1                  ← HTTP request
#   < HTTP/1.1 200 OK                 ← উত্তর

# ২. তিনটা সাইটের সনদের মেয়াদ বের করো
for d in google.com github.com example.com; do
  echo -n "$d: "
  echo | openssl s_client -connect $d:443 -servername $d 2>/dev/null \
    | openssl x509 -noout -enddate
done

# ৩. redirect chain দেখো
curl -sIL http://github.com | grep -E "^HTTP|^location"
```

**ছোট সমস্যা:** নিজের একটা self-signed সনদ বানিয়ে ত্রুটিটা দেখো।

```bash
openssl req -x509 -newkey rsa:2048 -nodes -keyout key.pem -out cert.pem \
  -days 1 -subj "/CN=localhost"

openssl s_server -accept 8443 -cert cert.pem -key key.pem &

curl https://localhost:8443        # ত্রুটি দেখবে — কী লেখা আছে পড়ো
curl -k https://localhost:8443     # এখন কাজ করবে

kill %1; rm -f key.pem cert.pem
```

<a id="s3-5"></a>

### ৩.৫ HTTP/2, HTTP/3, WebSocket, CORS — **[কাজচালানো]**

**HTTP/2** — একটা TCP সংযোগেই বহু request সমান্তরালে (multiplexing), header কম্প্রেস করা, binary ফরম্যাট। gRPC এর উপরেই দাঁড়ানো। জানা দরকার এটুকু: **ALB HTTP/2 সাপোর্ট করে, কিন্তু backend-এ HTTP/1.1-এ নামিয়ে দেয়** — তাই ALB-র পেছনে সরাসরি gRPC চালাতে গেলে আলাদা কনফিগ লাগে।

**HTTP/3 / QUIC** — TCP-র বদলে UDP-র উপর। মোবাইল নেটওয়ার্কে ভালো। **[নাম-জানা]** স্তরে রাখো।

**WebSocket** — একটা HTTP সংযোগকে "upgrade" করে দুইমুখী স্থায়ী চ্যানেল বানায়। DevOps-এ যা লাগে: LB-তে idle timeout বাড়াতে হবে (default ৬০ সেকেন্ডে সংযোগ কেটে যাবে), আর proxy-তে `Upgrade`/`Connection` header পাস করাতে হবে।

**CORS** — ব্রাউজারের নিরাপত্তা নিয়ম: এক origin-এর পেজ অন্য origin-এ AJAX করতে পারবে না, যদি না সার্ভার `Access-Control-Allow-Origin` header দিয়ে অনুমতি দেয়। এটা **শুধু ব্রাউজারে** ঘটে — `curl`-এ কখনো CORS ত্রুটি পাবে না। Frontend টিম "API কাজ করছে না" বলে আসবে, অথচ `curl` দিয়ে সব ঠিক দেখাবে — তখন বুঝবে CORS।

#### 🎤 ইন্টারভিউ প্রশ্ন

<details>
<summary>▸ ব্রাউজারে <code>https://example.com</code> লিখলে কী কী ঘটে? — ক্লাসিক প্রশ্ন</summary>

ধাপে ধাপে বলব:

১. **DNS** — ব্রাউজার cache → OS cache → `/etc/hosts` → resolver → root → TLD → authoritative। নাম থেকে IP পেলাম।

২. **TCP** — ওই IP-র ৪৪৩ পোর্টে 3-way handshake (SYN, SYN-ACK, ACK)।

৩. **TLS** — ClientHello (SNI-তে নামসহ) → ServerHello + certificate → সনদ যাচাই (CA চেইন, নামের মিল, মেয়াদ) → key exchange → এনক্রিপ্টেড চ্যানেল চালু।

৪. **HTTP** — `GET / HTTP/1.1` সাথে `Host` header। পথে load balancer থাকলে সে target group থেকে একটা healthy backend বাছে।

৫. **উত্তর** — status code সহ HTML ফেরত আসে, ব্রাউজার parse করে আরও resource (CSS/JS/image) চায়, সম্ভব হলে একই সংযোগে (keep-alive)।

ইন্টারভিউয়ার সাধারণত যেকোনো একটা ধাপে থামিয়ে গভীরে যেতে বলে — তাই প্রতিটা ধাপ আলাদা করে বলতে পারা জরুরি।

</details>

<details>
<summary>▸ 502 আর 504-এর পার্থক্য কী? কোনটা পেলে কোথায় খুঁজবে?</summary>

দুইটাই বলে সমস্যা proxy/LB-র **পেছনে**, কিন্তু ধরন আলাদা।

**502 Bad Gateway** — upstream-এর সাথে কথা বলা গেল না, বা সে অবৈধ উত্তর দিল। দেখব: অ্যাপ চলছে কিনা, ঠিক পোর্টে bind কিনা, target group-এ কোনো healthy target আছে কিনা, k8s-এ Service-এর পেছনে endpoint আছে কিনা (`kubectl get endpoints`), অ্যাপ crash loop-এ কিনা।

**504 Gateway Timeout** — upstream আছে, সংযোগও হলো, কিন্তু নির্ধারিত সময়ে উত্তর দিল না। দেখব: ধীর database query, বাইরের API-র জন্য অপেক্ষা, thread pool শেষ, অথবা LB-র idle timeout অ্যাপের স্বাভাবিক response time-এর চেয়ে কম।

এক লাইনে: **502 = কেউ নেই বা ভাঙা। 504 = আছে কিন্তু দেরি করছে।**

</details>

<details>
<summary>▸ HTTPS সাইটে ঢুকতে গিয়ে ব্রাউজার সনদের সতর্কবার্তা দিচ্ছে। কীভাবে ধরবে?</summary>

প্রথমেই যাচাই করব সমস্যাটা সত্যিই সনদে কিনা:

```bash
curl https://site.com      # ব্যর্থ
curl -k https://site.com   # সফল হলে → সমস্যা শুধুই সনদে
```

তারপর সনদটা দেখব:

```bash
echo | openssl s_client -connect site.com:443 -servername site.com 2>/dev/null \
  | openssl x509 -noout -subject -issuer -dates
```

চারটা সম্ভাব্য কারণ খুঁজব:
১. **মেয়াদ শেষ** — `notAfter` দেখে
২. **নামে মিল নেই** — `CN`/SAN-এ ঠিক domain আছে কিনা
৩. **চেইন অসম্পূর্ণ** — intermediate বাদ পড়েছে (`-showcerts` দিয়ে দেখব)। ব্রাউজারে কাজ করে কিন্তু curl-এ না হলে এটাই কারণ
৪. **Self-signed** — issuer আর subject একই

সবচেয়ে কমন কারণ প্রথমটা। আর স্থায়ী সমাধান হলো cert expiry-র উপর monitoring alert বসানো — ৩০ দিন আগে সতর্কতা।

</details>

<details>
<summary>▸ অ্যাপের লগে সব request-এর IP একই দেখাচ্ছে। কেন?</summary>

অ্যাপটা load balancer বা reverse proxy-র পেছনে আছে, তাই সে প্রতিটা সংযোগে LB-র IP দেখছে — আসল ব্যবহারকারীর না।

আসল IP থাকে `X-Forwarded-For` header-এ (একাধিক proxy থাকলে কমা দিয়ে আলাদা তালিকা, প্রথমটা আসল client)। সমাধান দুই দিকে: LB যেন header পাঠায় (ALB default-এ পাঠায়; NLB-তে Proxy Protocol লাগে), আর অ্যাপ যেন সেটা পড়ে — framework-এ trusted proxy সেটিং চালু করতে হয়।

সতর্কতা: header জাল করা যায়। তাই শুধু বিশ্বস্ত proxy থেকে আসা মান গ্রহণ করতে হবে, নইলে IP-ভিত্তিক rate limit বা ban ফাঁকি দেওয়া যাবে।

</details>

<details>
<summary>▸ "API-তে CORS error আসছে" — তুমি কী করবে?</summary>

প্রথমে বোঝাব CORS **ব্রাউজারের** নিয়ম, সার্ভারের ত্রুটি না। `curl` দিয়ে API ডাকলে কোনো সমস্যাই হবে না — কারণ curl এই নিয়ম মানে না।

দেখব সার্ভার ঠিক header পাঠাচ্ছে কিনা:

```bash
curl -I -H "Origin: https://frontend.com" https://api.backend.com/users
# Access-Control-Allow-Origin header আছে কিনা দেখব
```

তারপর preflight দেখব — non-simple request-এর আগে ব্রাউজার একটা `OPTIONS` request পাঠায়:

```bash
curl -X OPTIONS -H "Origin: https://frontend.com" \
     -H "Access-Control-Request-Method: POST" -I https://api.backend.com/users
```

সাধারণ কারণ: backend-এ CORS config নেই, অথবা `OPTIONS` request-টা LB/Ingress-এই আটকে যাচ্ছে (অনেক সময় auth middleware OPTIONS-কেও 401 দেয়)।

</details>

[↑ সূচিপত্রে ফিরে যাও](#index)

---

<a id="s4"></a>

## ৪. Linux networking

<a id="s4-1"></a>

### ৪.১ Interface ও routing table — **[গভীর]**

প্রতিটা প্যাকেট পাঠানোর আগে Linux একটা প্রশ্নের উত্তর খোঁজে: **"এই গন্তব্যে যেতে কোন দরজা দিয়ে বেরোব?"** উত্তরটা routing table-এ।

```bash
ip addr        # কোন ইন্টারফেসে কোন IP
ip route       # কোথায় যেতে কোন পথ
ip neigh       # কাছের কোন মেশিনের MAC জানা আছে (ARP table)
```

```bash
$ ip route
default via 10.0.1.1 dev eth0            ← বাকি সবকিছুর জন্য এই পথ
10.0.1.0/24 dev eth0 proto kernel scope link src 10.0.1.25
172.17.0.0/16 dev docker0 proto kernel scope link src 172.17.0.1
```

পড়ার নিয়ম — **উপর থেকে নিচে না, বরং সবচেয়ে নির্দিষ্ট মিলটা জেতে**:

- `10.0.1.50`-তে যেতে হলে → দ্বিতীয় লাইন মিলল, সরাসরি `eth0` দিয়ে
- `172.17.0.3`-এ যেতে হলে → তৃতীয় লাইন, `docker0` দিয়ে (এটা তোমার container)
- `8.8.8.8`-এ যেতে হলে → কোনো নির্দিষ্ট লাইন মিলল না → `default` পথে, gateway `10.0.1.1`-এর হাতে তুলে দাও

**Default gateway** হলো "যে ঠিকানা আমি চিনি না, সেগুলো এর কাছে পাঠাই"। AWS-এ এটা VPC router। বাসায় এটা তোমার রাউটার।

#### 🔧 হাতে-কলমে

```bash
# ১. তোমার default gateway কী?
ip route | grep default

# ২. একটা নির্দিষ্ট গন্তব্যে যেতে Linux কোন পথ বাছবে — সরাসরি জিজ্ঞেস করো
ip route get 8.8.8.8
ip route get 10.0.1.50

# ৩. Docker চালু থাকলে docker0 ইন্টারফেস খুঁজে বের করো
ip addr show docker0
```

<a id="s4-2"></a>

### ৪.২ NAT — private IP কীভাবে ইন্টারনেটে যায় — **[গভীর]**

তোমার মেশিনের IP `10.0.1.25` — একটা private ঠিকানা, ইন্টারনেটে রাউট হয় না। তাহলে `curl google.com` কীভাবে কাজ করে?

```
[তোমার মেশিন]          [NAT ডিভাইস]              [ইন্টারনেট]
 10.0.1.25    ──────▶   public IP: 52.1.2.3  ──────▶   google
 from: 10.0.1.25        from: 52.1.2.3
                        (একটা টেবিলে লিখে রাখে কে কী চেয়েছিল)
              ◀──────                         ◀──────
 to: 10.0.1.25          to: 10.0.1.25         উত্তর আসে 52.1.2.3-তে
```

NAT ডিভাইস বেরোনোর সময় source IP বদলে নিজেরটা বসায়, আর একটা টেবিলে লিখে রাখে কোন সংযোগ কার। উত্তর ফিরলে টেবিল দেখে আসল মেশিনে পাঠায়।

**দুই ধরনের NAT:**

| | কী বদলায় | কোথায় দেখবে |
|---|---|---|
| **SNAT** (source NAT) | বেরোনোর সময় **source** IP | NAT Gateway, বাসার রাউটার |
| **DNAT** (destination NAT) | ঢোকার সময় **destination** IP | port forwarding, `docker -p`, k8s Service |

**সবচেয়ে গুরুত্বপূর্ণ ফলাফল:** NAT একমুখী। ভেতর থেকে বাইরে যাওয়া যায়, কিন্তু বাইরে থেকে কেউ নিজে থেকে ভেতরে ঢুকতে পারে না — কারণ টেবিলে তার কোনো entry নেই। এজন্যই private subnet নিরাপদ।

<a id="s4-3"></a>

### ৪.৩ iptables সংক্ষেপে — **[কাজচালানো]**

লিখতে না পারলেও **পড়তে পারা** দরকার, কারণ Docker আর Kubernetes দুইটাই এর উপরে চলে।

```bash
sudo iptables -L -n -v          # filter rules (অনুমতি/নিষেধ)
sudo iptables -t nat -L -n      # NAT rules (ঠিকানা বদল)
```

তিনটা চেইন জানলেই চলবে:

| চেইন | কখন | উদাহরণ |
|---|---|---|
| `INPUT` | প্যাকেট **এই মেশিনের জন্য** এসেছে | firewall rule |
| `OUTPUT` | প্যাকেট **এই মেশিন থেকে** যাচ্ছে | |
| `FORWARD` | প্যাকেট **এই মেশিন দিয়ে অন্য কোথাও** যাচ্ছে | Docker, রাউটার |

#### 🔧 হাতে-কলমে — Docker আসলে কী করে দেখো

```bash
docker run -d -p 8080:80 nginx

# NAT টেবিলে Docker-এর বসানো DNAT নিয়ম খুঁজে বের করো
sudo iptables -t nat -L DOCKER -n
# দেখবে:  DNAT  tcp  dpt:8080 to:172.17.0.2:80
#          ↑ এটাই "-p 8080:80"-এর আসল রূপ

curl localhost:8080
docker rm -f $(docker ps -lq)
```

**এই এক পরীক্ষাটা করলে container networking-এর অর্ধেক রহস্য শেষ।** `-p` কোনো জাদু না — এটা শুধু একটা DNAT নিয়ম।

<a id="s4-4"></a>

### ৪.৪ Network namespace ও veth — **[কাজচালানো]**

Container-এর "নিজস্ব নেটওয়ার্ক" আসলে কোনো আলাদা মেশিন না। Linux kernel-এর একটা ফিচার — **network namespace**। প্রতিটা namespace-এর নিজের ইন্টারফেস, নিজের routing table, নিজের iptables।

দুইটা namespace-কে জোড়া লাগায় **veth pair** — একটা ভার্চুয়াল তার, যার এক মাথা container-এ (`eth0`), আরেক মাথা host-এ (`vethXXXX`), আর host-এর মাথাটা লাগানো থাকে `docker0` bridge-এ।

```
[container namespace]          [host namespace]
      eth0  ──────── veth pair ──────── veth123
   172.17.0.2                              │
                                       docker0 (bridge)
                                           │
                                         eth0 → বাইরের দুনিয়া
```

এই একটা ছবি মাথায় থাকলে Docker আর Kubernetes দুইটারই নেটওয়ার্ক বোঝা সহজ হয়ে যায়।

#### 🔧 হাতে-কলমে (ঐচ্ছিক, কিন্তু করলে অনেক পরিষ্কার হবে)

```bash
# নিজের হাতে একটা "container" বানাও
sudo ip netns add mynet
sudo ip netns list

# ভেতরে ঢুকে দেখো — সম্পূর্ণ আলাদা নেটওয়ার্ক জগৎ
sudo ip netns exec mynet ip addr      # শুধু lo, আর কিছুই নেই
sudo ip netns exec mynet ip route     # খালি

# পরিষ্কার করো
sudo ip netns del mynet
```

#### 🎤 ইন্টারভিউ প্রশ্ন

<details>
<summary>▸ Private subnet-এর একটা EC2 instance কীভাবে ইন্টারনেটে পৌঁছায়?</summary>

NAT Gateway-র মাধ্যমে।

private subnet-এর route table-এ `0.0.0.0/0` এর জন্য target হিসেবে NAT Gateway দেওয়া থাকে। NAT Gateway নিজে বসে থাকে একটা **public** subnet-এ, আর তার একটা Elastic IP আছে।

প্রবাহটা এমন: instance (`10.0.11.5`) → private subnet-এর route table → NAT Gateway → NAT source IP বদলে নিজের public IP বসায় → Internet Gateway → ইন্টারনেট। উত্তর ঠিক উল্টো পথে ফেরে, NAT নিজের টেবিল দেখে আসল instance-এ পাঠায়।

গুরুত্বপূর্ণ দিক: এটা **একমুখী**। ইন্টারনেট থেকে কেউ নিজে থেকে ওই instance-এ সংযোগ করতে পারবে না — এটাই private subnet ব্যবহারের মূল কারণ।

খরচের দিকটাও বলব: NAT Gateway ঘণ্টা হিসেবে + প্রতি GB হিসেবে টাকা নেয়। S3 বা ECR-এর মতো AWS service-এর ট্রাফিক VPC Endpoint দিয়ে পাঠালে সেই খরচ বাঁচে।

</details>

<details>
<summary>▸ NAT Gateway কেন public subnet-এ বসাতে হয়?</summary>

কারণ NAT Gateway-কে নিজেই ইন্টারনেটে পৌঁছাতে হবে। তার মানে তার subnet-এর route table-এ `0.0.0.0/0 → Internet Gateway` থাকতে হবে — আর সেই সংজ্ঞাই তো "public subnet"।

NAT Gateway-কে private subnet-এ বসালে সে নিজেই আটকা পড়বে, তখন কেউই বাইরে যেতে পারবে না।

</details>

<details>
<summary>▸ <code>docker run -p 8080:80</code> আসলে কী করে?</summary>

এটা host-এর ৮০৮০ পোর্টে আসা ট্রাফিককে container-এর ৮০ পোর্টে পাঠানোর জন্য একটা **DNAT নিয়ম** বসায় iptables-এর NAT টেবিলে।

`sudo iptables -t nat -L DOCKER -n` চালালে নিয়মটা চোখে দেখা যায় — `DNAT tcp dpt:8080 to:172.17.0.2:80`।

এজন্যই container-এর ভেতরে অ্যাপ `127.0.0.1`-এ bind করলে কাজ করে না: DNAT ট্রাফিক পাঠায় container-এর `eth0` ঠিকানায় (`172.17.0.2`), আর অ্যাপ সেখানে শুনছে না। অ্যাপকে `0.0.0.0`-তে bind করাতে হবে।

</details>

[↑ সূচিপত্রে ফিরে যাও](#index)

---

<a id="s5"></a>

## ৫. Troubleshooting — আসল কাজ

এই অধ্যায়টাই তোমাকে ইন্টারভিউতে আর চাকরিতে আলাদা করবে। বাকি সব অধ্যায় তথ্য, এটা দক্ষতা।

<a id="s5-1"></a>

### ৫.১ চার ধাপের runbook — **[গভীর]**

কেউ এসে বলল "সাইট কাজ করছে না"। এলোমেলোভাবে জিনিস চেক কোরো না। **সবসময় এই ক্রমে নামো:**

```
┌─ ধাপ ১: নাম থেকে IP পাচ্ছি? ──────────────────────────┐
│  dig +short site.com                                  │
│  getent hosts site.com                                │
│  ❌ না পেলে → DNS, /etc/hosts, record বদল, TTL        │
└───────────────────────────────────────────────────────┘
                     ↓ পেলে
┌─ ধাপ ২: ওই IP-র পোর্টে সংযোগ হচ্ছে? ─────────────────┐
│  nc -zv <ip> 443                                      │
│  ❌ timeout  → Security Group, NACL, firewall, route   │
│  ❌ refused  → service বন্ধ, ভুল পোর্ট, ভুল bind       │
└───────────────────────────────────────────────────────┘
                     ↓ হচ্ছে
┌─ ধাপ ৩: TLS handshake হচ্ছে? ────────────────────────┐
│  curl -v https://site.com                             │
│  curl -k https://site.com   ← এটা কাজ করলে সনদের দোষ  │
│  ❌ → মেয়াদ, নামের মিল, অসম্পূর্ণ চেইন, SNI           │
└───────────────────────────────────────────────────────┘
                     ↓ হচ্ছে
┌─ ধাপ ৪: HTTP উত্তর কী বলছে? ─────────────────────────┐
│  curl -i https://site.com                             │
│  4xx → request-এর দোষ (path, auth, header)            │
│  502 → upstream মরা / ভাঙা                            │
│  503 → কোনো healthy backend নেই                        │
│  504 → upstream ধীর                                    │
└───────────────────────────────────────────────────────┘
```

**কেন এই ক্রম:** প্রতিটা ধাপ আগেরটার উপর দাঁড়ানো। DNS ভাঙা থাকলে TLS দেখে সময় নষ্ট করার মানে নেই। এই শৃঙ্খলাটাই সিনিয়র আর জুনিয়রের পার্থক্য।

**আর একটা প্রশ্ন সবার আগে করো:** "সবার জন্য ভাঙা, নাকি শুধু তোমার জন্য?" আর "কখন থেকে? তার আগে কী deploy হয়েছে?" — অর্ধেক incident-এর উত্তর এই দুইটা প্রশ্নেই বেরিয়ে আসে।

<a id="s5-2"></a>

### ৫.২ টুলবক্স — **[গভীর]**

| টুল | কী দেখায় | কখন |
|---|---|---|
| `curl -v` | DNS + TCP + TLS + HTTP — চারটাই একসাথে | **প্রথমেই এটা** |
| `dig` | DNS-এর সব | নাম resolve হচ্ছে না |
| `getent hosts` | অ্যাপ আসলে কী দেখছে (hosts সহ) | dig ঠিক কিন্তু অ্যাপ ভুল |
| `ss -tulpn` | কোন পোর্টে কে শুনছে | "পোর্ট খোলা তো?" |
| `nc -zv` | দূরের পোর্টে পৌঁছানো যায় কিনা | firewall সন্দেহ |
| `ping` | মেশিন জীবিত + latency | cloud-এ ICMP বন্ধ থাকে, তাই সীমিত |
| `mtr` | প্রতিটা hop-এ packet loss | "মাঝে মাঝে স্লো" |
| `traceroute` | কোন পথে যাচ্ছে | রুটিং সন্দেহ |
| `openssl s_client` | সনদের বিস্তারিত | TLS সমস্যা |
| `tcpdump` | আসল প্যাকেট | অন্য সব যখন ব্যর্থ |
| `ip route get` | Linux কোন পথ বাছবে | রুটিং যাচাই |

#### `curl -v` পড়তে শেখো — সবচেয়ে বেশি দামি দক্ষতা

```bash
curl -v https://example.com
```

```
* Trying 93.184.216.34:443...              ← ✅ DNS কাজ করেছে
* Connected to example.com (93.184.216.34) ← ✅ TCP কাজ করেছে
* TLSv1.3 (OUT), TLS handshake             ← TLS শুরু
* Server certificate:
*  subject: CN=example.com                 ← ✅ সনদ ঠিক নামে
*  expire date: Mar 10 2027 GMT            ← ✅ মেয়াদ আছে
> GET / HTTP/1.1                           ← request গেল
> Host: example.com
< HTTP/1.1 200 OK                          ← ✅ উত্তর এলো
```

**যে লাইনে গিয়ে থেমে যায়, সমস্যাটা ঠিক সেখানে।** `Trying`-এ আটকালে DNS হয়েছে কিন্তু TCP হয়নি। `Connected` এসে থামলে TLS-এ সমস্যা। এইটুকু পড়তে পারলে তুমি বেশিরভাগ সমস্যা একাই ধরতে পারবে।

কাজের অপশন:

```bash
curl -I url                      # শুধু header
curl -L url                      # redirect অনুসরণ করো
curl -k url                      # সনদ যাচাই বাদ (ডিবাগে)
curl -m 5 url                    # ৫ সেকেন্ডের বেশি অপেক্ষা না
curl --resolve site.com:443:1.2.3.4 https://site.com   # DNS বাইপাস করে নির্দিষ্ট IP-তে
curl -w "\ntime: %{time_total}s dns: %{time_namelookup}s tcp: %{time_connect}s\n" -o /dev/null -s url
```

> শেষের `--resolve`-টা সোনার মতো কাজের: DNS বদলানোর **আগেই** নতুন সার্ভার ঠিকঠাক কাজ করছে কিনা পরীক্ষা করে নেওয়া যায়।

<a id="s5-3"></a>

### ৫.৩ tcpdump — **[কাজচালানো]**

যখন সব টুল বলছে "ঠিক আছে" অথচ কাজ করছে না, তখন প্যাকেট নিজে চোখে দেখতে হয়।

```bash
# একটা পোর্টের ট্রাফিক দেখো
sudo tcpdump -i any port 443 -n

# একটা নির্দিষ্ট মেশিনের সাথে কথা
sudo tcpdump -i any host 10.0.1.5 -n

# একসাথে দুই শর্ত
sudo tcpdump -i any 'host 10.0.1.5 and port 80' -n

# ফাইলে রেকর্ড করো (পরে Wireshark-এ দেখতে)
sudo tcpdump -i any port 443 -w capture.pcap
```

`-n` দিতে ভুলবে না — নইলে প্রতিটা IP-র জন্য DNS lookup করবে আর আউটপুট ধীর হয়ে যাবে।

**সবচেয়ে কাজের ব্যবহার:** "আমার request কি ওই সার্ভারে আদৌ পৌঁছাচ্ছে?" — সার্ভারে `tcpdump` চালিয়ে রাখো, তারপর client থেকে request পাঠাও।

- কিছুই দেখা গেল না → প্যাকেট পথেই মরছে (firewall, routing, ভুল IP)
- `SYN` দেখা গেল কিন্তু `SYN-ACK` নেই → প্যাকেট পৌঁছেছে, কিন্তু সার্ভার উত্তর দিচ্ছে না বা উত্তর ব্লক হচ্ছে

#### 🔧 হাতে-কলমে — নিজের হাতে তিনটা সমস্যা বানাও আর ধরো

**সমস্যা ১ — DNS ভাঙা**

```bash
echo "1.2.3.4 example.com" | sudo tee -a /etc/hosts
curl -v -m 5 http://example.com
# চার ধাপের runbook চালাও। কোন ধাপে ধরা পড়ল?
sudo sed -i '/1.2.3.4 example.com/d' /etc/hosts
```

**সমস্যা ২ — পোর্ট ব্লক (timeout)**

```bash
python3 -m http.server 8000 &
curl -m 3 http://127.0.0.1:8000        # কাজ করছে

sudo iptables -A INPUT -p tcp --dport 8000 -j DROP
curl -m 3 http://127.0.0.1:8000        # এখন? — timeout না refused?

sudo iptables -D INPUT -p tcp --dport 8000 -j DROP
kill %1
```

**সমস্যা ৩ — ভুল bind (refused)**

```bash
python3 -m http.server 8000 --bind 127.0.0.1 &
MYIP=$(hostname -I | awk '{print $1}')
curl -m 3 http://127.0.0.1:8000        # কাজ করে
curl -m 3 http://$MYIP:8000            # কাজ করে না — কোন ত্রুটি?
kill %1
```

তিনটা শেষ করে একটা টেবিল বানাও: *কোন সমস্যা → কোন ত্রুটি বার্তা → কোন ধাপে ধরা পড়ল*। এই টেবিলটাই তোমার আসল শেখা।

#### 🎤 ইন্টারভিউ প্রশ্ন

<details>
<summary>▸ "আমাদের সাইট ডাউন" — তুমি কী করবে? (সবচেয়ে কমন open-ended প্রশ্ন)</summary>

আগে দুইটা প্রশ্ন করব, কারণ এতে খোঁজার জায়গা অর্ধেক হয়ে যায়:
- **সবার জন্য ডাউন, নাকি কিছু ব্যবহারকারীর জন্য?** (সবার হলে কেন্দ্রীয় কিছু; কিছু জনের হলে DNS propagation, একটা AZ, বা কয়েকটা unhealthy target)
- **কখন থেকে? তার ঠিক আগে কী বদলেছে?** (deploy, config, DNS, সনদের মেয়াদ)

তারপর বাইরে থেকে ভেতরের দিকে নামব:

১. `dig +short site.com` — নাম ঠিক IP দিচ্ছে?
২. `nc -zv <ip> 443` — পোর্ট খোলা? (timeout হলে firewall/SG, refused হলে service)
৩. `curl -v https://site.com` — TLS পেরোচ্ছে? `curl -k` কাজ করলে সনদের সমস্যা
৪. status code দেখব — 502/503 হলে LB-র target group বা k8s endpoint দেখব, 504 হলে backend-এর latency দেখব
৫. পাশাপাশি: monitoring dashboard, সাম্প্রতিক deploy, error rate, আর অ্যাপের লগ

সারাক্ষণ একটা নীতি ধরে রাখব — **অনুমান না করে প্রতিটা ধাপ আলাদা করে যাচাই করব**, আর যা যা বাদ দিয়ে দিলাম সেটা লিখে রাখব, যেন ঘুরেফিরে একই জায়গা না দেখি।

আর সবার আগে: সম্ভব হলে সমস্যাটা থামানো (রোলব্যাক), তারপর কারণ খোঁজা। ব্যবহারকারী অপেক্ষা করছে।

</details>

<details>
<summary>▸ Pod থেকে একটা external API-তে পৌঁছানো যাচ্ছে না, কিন্তু node থেকে যাচ্ছে। কোথায় খুঁজবে?</summary>

পার্থক্যটাই সূত্র — সমস্যা node-এর বাইরে না, pod-এর নেটওয়ার্ক পথে।

দেখব:
১. **NetworkPolicy** — egress deny করা আছে কিনা। DNS (৫৩ পোর্ট) বন্ধ করে দেওয়া খুব কমন ভুল
২. **DNS** — pod-এর ভেতর থেকে `nslookup api.example.com` চালাব; CoreDNS ঠিকমতো কাজ করছে কিনা
৩. **ndots** — external নাম resolve-এ বাড়তি চেষ্টা হচ্ছে কিনা
৪. **Service mesh** — Istio/Linkerd থাকলে sidecar external ট্রাফিক আটকাচ্ছে কিনা (ServiceEntry লাগতে পারে)
৫. **SNAT/CNI** — pod-এর ট্রাফিক node-এর IP-তে অনুবাদ হচ্ছে কিনা

ডিবাগের জন্য pod-এর ভেতরে টুলসহ একটা shell নেব:

```bash
kubectl run tmp --rm -it --image=nicolaka/netshoot -- bash
```

ওখান থেকে `dig`, `curl -v`, `nc -zv` চালিয়ে চার ধাপের runbook-টাই আবার চালাব — এবার pod-এর দৃষ্টিকোণ থেকে।

</details>

[↑ সূচিপত্রে ফিরে যাও](#index)

---

<a id="s6"></a>

## ৬. Cloud networking (AWS)

<a id="s6-1"></a>

### ৬.১ VPC-র শরীর — **[গভীর]**

VPC = AWS-এর ভেতরে তোমার নিজের একটা আলাদা নেটওয়ার্ক। ছবিটা মাথায় গেঁথে নাও:

```
┌─ VPC  10.0.0.0/16 ──────────────────────────────────────────┐
│                                                              │
│  ┌─ Public subnet 10.0.1.0/24 (AZ-a) ──┐                     │
│  │   ALB          NAT Gateway          │                     │
│  │   route: 0.0.0.0/0 → IGW            │                     │
│  └──────────────────────────────────────┘                    │
│                                                              │
│  ┌─ Private subnet 10.0.11.0/24 (AZ-a) ─┐                    │
│  │   App server      Database           │                    │
│  │   route: 0.0.0.0/0 → NAT Gateway     │                    │
│  └──────────────────────────────────────┘                    │
│                                                              │
└──────────────────┬───────────────────────────────────────────┘
                   │
            Internet Gateway (IGW)
                   │
               ইন্টারনেট
```

| অংশ | কাজ |
|---|---|
| **VPC** | পুরো নেটওয়ার্কের সীমানা, একটা CIDR |
| **Subnet** | VPC-র ভেতরে একটা টুকরা, **একটা মাত্র AZ-তে থাকে** |
| **Route table** | কোন ট্রাফিক কোথায় যাবে — এটাই ঠিক করে subnet public না private |
| **Internet Gateway** | VPC-কে ইন্টারনেটে যুক্ত করে, একটা VPC-তে একটাই |
| **NAT Gateway** | private subnet-কে বাইরে যেতে দেয়, ভেতরে আসতে দেয় না |
| **Security Group** | instance-এর গায়ে লাগানো firewall |
| **NACL** | subnet-এর গায়ে লাগানো firewall |

> **High availability-র নিয়ম:** subnet একটা AZ-তেই থাকে। তাই প্রতিটা tier-এর জন্য অন্তত **দুইটা AZ-তে দুইটা subnet** বানাও। এক AZ পড়ে গেলেও অ্যাপ চলবে।

<a id="s6-2"></a>

### ৬.২ Public vs private subnet — **[গভীর]**

AWS-এ "public subnet" নামে আলাদা কোনো সুইচ নেই। পার্থক্যটা শুধু **route table**-এ:

```
Public subnet-এর route table          Private subnet-এর route table
────────────────────────────          ─────────────────────────────
10.0.0.0/16  →  local                 10.0.0.0/16  →  local
0.0.0.0/0    →  igw-xxxxx             0.0.0.0/0    →  nat-xxxxx
                 ↑ এটাই পার্থক্য                        ↑
```

**কোথায় কী বসাবে:**

| Subnet | কী বসাবে |
|---|---|
| Public | ALB/NLB, NAT Gateway, bastion host |
| Private | অ্যাপ সার্ভার, database, cache, k8s worker node |

**নিয়ম:** যেটার public IP লাগে না, সেটা private-এ। Database কখনো public subnet-এ না।

<a id="s6-3"></a>

### ৬.৩ Security Group vs NACL — **[গভীর]**

এটা AWS ইন্টারভিউয়ের চিরস্থায়ী প্রশ্ন। টেবিলটা মুখস্থ করে ফেলো:

| | Security Group | NACL |
|---|---|---|
| কোথায় লাগে | instance/ENI-তে | subnet-এ |
| নিয়মের ধরন | শুধু **allow** | **allow এবং deny** দুইটাই |
| State | **Stateful** | **Stateless** |
| মূল্যায়ন | সব নিয়ম একসাথে দেখে | নম্বর অনুযায়ী উপর থেকে নিচে, প্রথম মিলেই শেষ |
| Default | সব inbound বন্ধ, সব outbound খোলা | সব খোলা |

#### Stateful মানে কী — এটাই আসল পার্থক্য

**Security Group (stateful):** তুমি inbound-এ ৪৪৩ খুললে, উত্তরটা আপনাআপনি বেরোতে পারবে। outbound নিয়ম লেখার দরকার নেই। সে মনে রাখে কোন সংযোগ সে ঢুকতে দিয়েছে।

**NACL (stateless):** সে কিছুই মনে রাখে না। inbound-এ ৪৪৩ খুললে উত্তর বেরোনোর জন্য **outbound-এ ephemeral port range (1024–65535) আলাদা করে খুলতে হবে**। এটা ভুলে যাওয়া NACL-এর সবচেয়ে কমন ভুল।

#### একটা বাস্তব প্যাটার্ন — SG-কে SG-র ভেতরে রেফার করা

```
ALB-এর SG        : inbound 443 ← 0.0.0.0/0
App server-এর SG : inbound 8080 ← sg-alb          (IP না, SG-র নাম!)
Database-এর SG   : inbound 5432 ← sg-app
```

এতে IP মনে রাখতে হয় না, auto-scaling-এ নতুন instance এলেও নিয়ম ঠিক থাকে, আর নিরাপত্তার স্তরগুলো নামেই পরিষ্কার বোঝা যায়। **প্রোডাকশনে সবসময় এভাবে করো।**

<a id="s6-4"></a>

### ৬.৪ Load balancer — ALB vs NLB — **[গভীর]**

| | ALB (L7) | NLB (L4) |
|---|---|---|
| কী বোঝে | HTTP/HTTPS — path, header, host | শুধু TCP/UDP — IP আর port |
| রাউটিং | path/host দেখে আলাদা target-এ | শুধু forward |
| গতি | ভালো | অনেক দ্রুত, খুব কম latency |
| স্থির IP | না (নাম ব্যবহার করতে হয়) | হ্যাঁ (Elastic IP দেওয়া যায়) |
| Client IP | `X-Forwarded-For` header-এ | সরাসরি সংরক্ষণ করে |
| TLS | নিজে শেষ করতে পারে | pass-through বা terminate |
| কখন | ওয়েব অ্যাপ, API, microservice | gRPC, MQTT, database proxy, স্থির IP দরকার হলে |

**Target group ও health check** — LB শুধু "healthy" target-এ ট্রাফিক পাঠায়। health check ভুল হলে দুইরকম বিপদ:

- health check path ভুল (যেমন `/` দিলে কিন্তু অ্যাপ সেখানে 404 দেয়) → সব target unhealthy → **503**
- health check খুব সহজ (শুধু পোর্ট খোলা কিনা দেখে) → অ্যাপ ভেতরে ভাঙা থাকলেও ট্রাফিক পাবে → **502**

তাই অ্যাপে একটা আলাদা `/healthz` endpoint রাখো, যেটা সত্যিই নির্ভরশীল জিনিসগুলো (DB সংযোগ) পরীক্ষা করে।

<a id="s6-5"></a>

### ৬.৫ VPC Flow Logs — **[গভীর]**

Cloud-এ `tcpdump` চালানোর সুযোগ সবসময় থাকে না। Flow Logs সেই ফাঁকটা পূরণ করে — কোন ট্রাফিক গ্রহণ হলো আর কোনটা ফেলে দেওয়া হলো, তার রেকর্ড।

```
2 123456789 eni-abc 10.0.1.5 10.0.11.20 45678 5432 6 10 840 1699... 1699... REJECT OK
              │        │         │        │     │   │                          │
           account   source    dest     sport dport proto                   ACCEPT/REJECT
```

**যা খুঁজবে:** `REJECT`। কোন source থেকে কোন dest-এর কোন পোর্টে REJECT হচ্ছে দেখলেই বুঝবে কোন SG বা NACL আটকাচ্ছে।

> "Timeout হচ্ছে কিন্তু কেন বুঝছি না" — cloud-এ এই সমস্যার সবচেয়ে সরাসরি উত্তর Flow Logs। VPC-তে এটা আগেই চালু করে রাখা ভালো অভ্যাস।

<a id="s6-6"></a>

### ৬.৬ Peering, Endpoint, VPN, TGW — **[কাজচালানো]**

| জিনিস | কাজ | যা মনে রাখতে হবে |
|---|---|---|
| **VPC Peering** | দুইটা VPC-কে জোড়া | **transitive না** — A↔B, B↔C থাকলেও A↔C হবে না। CIDR overlap করলে সম্ভব না |
| **VPC Endpoint** | AWS service-এ private পথে যাওয়া | NAT খরচ বাঁচায়। Gateway type (S3, DynamoDB) ফ্রি; Interface type (বাকি সব) ঘণ্টাপ্রতি খরচ |
| **Site-to-Site VPN** | অফিস ↔ VPC | ইন্টারনেটের উপর দিয়ে, তাই গতি নিশ্চিত না |
| **Transit Gateway** | অনেক VPC-র কেন্দ্রীয় হাব | peering-এর জাল (n²) এড়ায়। **[নাম-জানা]** স্তরে রাখলেই হবে শুরুতে |
| **Direct Connect** | ডেটা সেন্টার ↔ AWS ডেডিকেটেড লাইন | দামি, বড় প্রতিষ্ঠানে |

#### 🔧 হাতে-কলমে — একটা VPC নিজের হাতে বানাও

শুধু console-এ ক্লিক করে না, **প্রতিটা অংশ আলাদা করে** বানাও, যাতে বুঝতে পারো কোনটা কী করে:

```
১.  VPC বানাও          — 10.0.0.0/16
২.  ৪টা subnet          — 10.0.1.0/24, 10.0.2.0/24 (public, দুই AZ)
                          10.0.11.0/24, 10.0.12.0/24 (private, দুই AZ)
৩.  Internet Gateway    — বানাও, VPC-তে যুক্ত করো
৪.  Public route table  — 0.0.0.0/0 → IGW, public subnet-দুইটায় যুক্ত করো
৫.  NAT Gateway         — public subnet-এ, একটা Elastic IP সহ
৬.  Private route table — 0.0.0.0/0 → NAT, private subnet-দুইটায় যুক্ত করো
৭.  একটা EC2 public-এ, একটা private-এ
৮.  public-টায় SSH করো, সেখান থেকে private-টায় SSH করো
৯.  private EC2 থেকে চালাও: curl ifconfig.me
    → কোন IP দেখাচ্ছে? কেন সেটা NAT Gateway-র IP?
১০. NAT Gateway মুছে দাও, আবার curl চালাও → timeout দেখবে
```

> **সতর্কতা:** শেখা শেষে NAT Gateway আর Elastic IP অবশ্যই মুছে দাও। এই দুইটা ফেলে রাখলে মাসে $৩০-৪০ কেটে নেবে।

**ছোট সমস্যা:** private subnet-এ একটা EC2-তে `apt update` চালাও। কাজ না করলে চার ধাপের runbook দিয়ে কারণ বের করো।

<details>
<summary>▸ ইঙ্গিত</summary>

এই ক্রমে দেখো:
১. private subnet-এর route table-এ `0.0.0.0/0 → nat-xxx` আছে?
২. NAT Gateway কি **public** subnet-এ বসানো? (private-এ বসালে সে নিজেই আটকা)
৩. NAT-এর subnet-এর route table-এ `0.0.0.0/0 → igw-xxx` আছে?
৪. instance-এর SG-তে outbound খোলা? (default-এ খোলা থাকে)
৫. NACL-এ outbound 80/443 আর **inbound ephemeral port (1024-65535)** খোলা? (stateless, মনে আছে?)
৬. DNS কাজ করছে? VPC-র `enableDnsSupport` চালু আছে?

</details>

#### 🎤 ইন্টারভিউ প্রশ্ন

<details>
<summary>▸ Security Group আর NACL-এর পার্থক্য কী?</summary>

তিনটা বড় পার্থক্য:

**১. কোথায় বসে** — SG instance/ENI-র গায়ে, NACL subnet-এর সীমানায়। মানে একই subnet-এর দুইটা instance-এর মধ্যে ট্রাফিক NACL দেখেই না, কিন্তু SG দেখে।

**২. Stateful vs stateless** — SG stateful: inbound-এ অনুমতি দিলে উত্তর আপনাআপনি বেরোতে পারে। NACL stateless: প্রতিটা দিক আলাদা করে খুলতে হয়, তাই উত্তরের জন্য outbound-এ ephemeral port (1024–65535) খোলা লাগে। এটা ভুলে যাওয়া খুব কমন বাগ।

**৩. Deny করা যায় কিনা** — SG-তে শুধু allow লেখা যায়, তাই "এই IP-কে ব্লক করো" SG দিয়ে সম্ভব না। NACL-এ deny লেখা যায়, তাই নির্দিষ্ট IP ব্লক করতে NACL লাগে।

বাস্তবে: দৈনন্দিন নিয়ন্ত্রণ SG দিয়েই করি (বিশেষ করে SG-কে SG-র মধ্যে রেফার করে), আর NACL রাখি মোটা দাগের subnet-স্তরের নিয়মে — যেমন কোনো খারাপ IP রেঞ্জ ব্লক করা।

</details>

<details>
<summary>▸ VPC peering কি transitive?</summary>

না। A↔B আর B↔C peering থাকলেও A থেকে C-তে যাওয়া যাবে না। প্রতিটা জোড়ার জন্য আলাদা peering আর দুই পাশে আলাদা route লাগে।

তাই VPC-র সংখ্যা বাড়লে peering-এর সংখ্যা n(n−1)/2 হারে বাড়ে — ১০টা VPC মানে ৪৫টা peering, যা ব্যবস্থাপনার অযোগ্য। এই জায়গায় **Transit Gateway** ব্যবহার করা হয়, যেখানে প্রতিটা VPC শুধু একবার হাবের সাথে যুক্ত হয়।

আরেকটা সীমাবদ্ধতা: CIDR overlap করলে peering সম্ভবই না — এজন্যই শুরুতে CIDR পরিকল্পনা এত গুরুত্বপূর্ণ।

</details>

<details>
<summary>▸ ALB আর NLB-র মধ্যে কোনটা কখন নেবে?</summary>

**ALB** নেব যখন HTTP-স্তরের বুদ্ধি লাগবে — path দেখে `/api` এক service-এ আর `/app` আরেক service-এ পাঠানো, host-based routing, HTTP header দেখে সিদ্ধান্ত, WAF যুক্ত করা, বা TLS ALB-তেই শেষ করা। সাধারণ ওয়েব অ্যাপ আর API-র জন্য এটাই ডিফল্ট।

**NLB** নেব যখন — (ক) স্থির IP দরকার, যেমন ক্লায়েন্ট তাদের firewall-এ আমাদের IP whitelist করবে, (খ) HTTP না এমন প্রোটোকল (MQTT, database, gRPC-র কিছু ক্ষেত্র), (গ) খুব কম latency আর অনেক বেশি throughput দরকার, (ঘ) client-এর আসল IP header ছাড়াই দরকার।

খরচেও পার্থক্য আছে, কিন্তু সিদ্ধান্তটা সাধারণত "HTTP-স্তরের রাউটিং লাগবে কি লাগবে না" — এই প্রশ্নেই নির্ধারিত হয়।

</details>

<details>
<summary>▸ ALB থেকে 503 আসছে। কী দেখবে?</summary>

503 মানে সাধারণত **কোনো healthy target নেই**। তাই সবার আগে target group-এ গিয়ে target-গুলোর অবস্থা দেখব।

সব unhealthy হলে প্রায় সবসময় health check-এর সমস্যা:
- health check **path** ঠিক? (`/` দিয়েছি কিন্তু অ্যাপ সেখানে 404 দেয়?)
- health check **port** ঠিক? অ্যাপ আসলে কোন পোর্টে শুনছে?
- target-এর SG কি ALB-র SG থেকে ওই পোর্টে inbound অনুমতি দেয়?
- অ্যাপ কি health check-এর timeout-এর মধ্যে উত্তর দিতে পারছে? (ঠান্ডা অবস্থায় ধীর শুরু হলে `HealthCheckGracePeriod` বাড়াতে হয়)
- অ্যাপ কি `0.0.0.0`-তে bind হয়েছে, নাকি `127.0.0.1`-এ?

target registered-ই না হলে দেখব auto scaling group বা ECS/EKS service ঠিকমতো target group-এ যুক্ত হচ্ছে কিনা।

</details>

[↑ সূচিপত্রে ফিরে যাও](#index)

---

<a id="s7"></a>

## ৭. Container ও Kubernetes networking

<a id="s7-1"></a>

### ৭.১ Docker networking — **[গভীর]**

Docker-এর তিনটা মোড জানলেই শুরুর জন্য যথেষ্ট:

| মোড | কী হয় | কখন |
|---|---|---|
| `bridge` (default) | container নিজের IP পায়, NAT দিয়ে বাইরে যায় | প্রায় সবসময় |
| `host` | host-এর নেটওয়ার্ক সরাসরি ব্যবহার করে, আলাদা IP নেই | খুব কম latency দরকার হলে |
| `none` | কোনো নেটওয়ার্ক নেই | সম্পূর্ণ বিচ্ছিন্ন কাজ |

#### দুইটা জিনিস যা beginner-দের সবচেয়ে বেশি আটকায়

**১. `-p 8080:80` মানে DNAT।** কোনো জাদু না — iptables-এ একটা নিয়ম বসে (৪.৩-এ নিজের চোখে দেখেছো)।

**২. container-এর ভেতরে `localhost` মানে সেই container নিজেই।** অ্যাপ `127.0.0.1`-এ bind করলে port mapping কাজ করবে না। `0.0.0.0`-তে bind করাও।

#### Container-রা একে অপরকে কীভাবে খুঁজে পায়

Default bridge-এ নাম দিয়ে খুঁজে পাওয়া যায় না — শুধু IP দিয়ে। কিন্তু **নিজের বানানো network**-এ Docker-এর নিজস্ব DNS চালু হয়ে যায়, তখন container-এর নাম দিয়েই ডাকা যায়:

```bash
docker network create mynet
docker run -d --name db --network mynet postgres:16-alpine
docker run --rm -it --network mynet alpine sh

# ভেতর থেকে:
ping db          # নাম দিয়েই কাজ করছে!
nslookup db
```

**এজন্যই docker-compose-এ service-এর নাম দিয়ে একে অপরকে ডাকা যায়** — compose নিজে থেকেই একটা custom network বানায়। এই এক তথ্যেই compose-এর অনেক রহস্য শেষ।

#### 🔧 হাতে-কলমে

```bash
# ১. bridge মোডে container-এর IP দেখো
docker run -d --name web nginx
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' web

# ২. host থেকে সরাসরি ওই IP-তে যাও (port mapping ছাড়াই!)
curl http://<সেই IP>

# ৩. custom network-এ নাম দিয়ে খোঁজা
docker network create labnet
docker run -d --name api --network labnet nginx
docker run --rm --network labnet curlimages/curl curl -s http://api

# পরিষ্কার
docker rm -f web api; docker network rm labnet
```

<a id="s7-2"></a>

### ৭.২ Kubernetes-এর নেটওয়ার্ক মডেল — **[গভীর]**

k8s-এর নেটওয়ার্ক তিনটা নিয়মের উপর দাঁড়ানো। এই তিনটা বুঝলে বাকিটা সহজ:

1. **প্রতিটা pod-এর নিজের একটা IP আছে**
2. **যেকোনো pod যেকোনো pod-এর সাথে NAT ছাড়া সরাসরি কথা বলতে পারে** — এমনকি অন্য node-এর pod হলেও
3. **Node-ও যেকোনো pod-এর সাথে সরাসরি কথা বলতে পারে**

এই "flat network" মডেলটাই k8s-কে Docker থেকে আলাদা করে। এটা বাস্তবায়ন করে **CNI plugin** (Calico, Flannel, Cilium, AWS VPC CNI)।

#### তিনটা আলাদা IP রেঞ্জ — গুলিয়ে ফেলো না

```
Node CIDR     10.0.0.0/16     ← VPC-র subnet, node-এর IP
Pod CIDR      172.20.0.0/16   ← pod-রা এখান থেকে IP পায়
Service CIDR  10.96.0.0/12    ← ClusterIP-রা এখান থেকে, কাল্পনিক IP
```

**তিনটাই একে অপরের সাথে এবং VPC-র সাথে overlap-মুক্ত হতে হবে।** ক্লাস্টার বানানোর আগে এটা পরিকল্পনা করা লাগে — পরে বদলাতে গেলে পুরো ক্লাস্টার নতুন করে বানাতে হয়।

> **মজার তথ্য:** ClusterIP আসলে কোনো মেশিনের ঠিকানা না। ওটা একটা ভার্চুয়াল IP — কোনো ইন্টারফেসে খুঁজে পাবে না, ping-ও করতে পারবে না। ওটা শুধু iptables/IPVS নিয়মে বেঁচে আছে।

<a id="s7-3"></a>

### ৭.৩ Service-এর ধরন — **[গভীর]**

Pod মরে যায়, নতুন pod নতুন IP পায়। তাহলে একটা স্থির ঠিকানা কোথায়? — **Service**।

| ধরন | কে পৌঁছাতে পারে | কীভাবে কাজ করে |
|---|---|---|
| **ClusterIP** (default) | শুধু ক্লাস্টারের ভেতর থেকে | একটা ভার্চুয়াল IP + DNS নাম |
| **NodePort** | ক্লাস্টারের বাইরে থেকে, `<nodeIP>:30000-32767` | প্রতিটা node-এ একটা পোর্ট খোলে |
| **LoadBalancer** | ইন্টারনেট থেকে | cloud-এ একটা আসল LB বানায় |
| **ExternalName** | — | শুধু একটা CNAME, বাইরের service-এর দিকে |

#### Service pod খুঁজে পায় কীভাবে — এটাই সবচেয়ে বেশি ভাঙে

```yaml
apiVersion: v1
kind: Service
metadata:
  name: api
spec:
  selector:
    app: api          # ← এই label যেসব pod-এ আছে
  ports:
    - port: 80        # ← Service-এর পোর্ট
      targetPort: 8080 # ← pod-এর পোর্ট
```

Service → **selector** দিয়ে pod খোঁজে → মিলে যাওয়া pod-গুলোর IP দিয়ে একটা **Endpoints** বস্তু বানায় → ট্রাফিক সেখানে পাঠায়।

**তাই ডিবাগের প্রথম কমান্ড সবসময় এটা:**

```bash
kubectl get endpoints api
```

খালি দেখালে বুঝবে selector কোনো pod-এর সাথে মেলেনি — label-এ টাইপো, অথবা pod এখনো Ready না। **k8s-এ "Service কাজ করছে না" সমস্যার সিংহভাগ এখানেই ধরা পড়ে।**

<a id="s7-4"></a>

### ৭.৪ CoreDNS — **[গভীর]**

ক্লাস্টারের ভেতরে প্রতিটা Service-এর একটা DNS নাম আছে:

```
<service>.<namespace>.svc.cluster.local

api.default.svc.cluster.local     ← পুরো নাম (FQDN)
api.default                        ← অন্য namespace থেকে
api                                ← একই namespace থেকে
```

`search` domain-এর কারণেই শেষেরটা কাজ করে (২.৪-এ দেখেছো)।

```bash
# pod-এর resolv.conf দেখো
kubectl exec -it <pod> -- cat /etc/resolv.conf
# nameserver 10.96.0.10                 ← CoreDNS-এর ClusterIP
# search default.svc.cluster.local svc.cluster.local cluster.local
# options ndots:5

# ভেতর থেকে DNS পরীক্ষা করো
kubectl run tmp --rm -it --image=busybox:1.36 -- nslookup api.default
```

<a id="s7-5"></a>

### ৭.৫ Ingress — **[গভীর]**

প্রতিটা service-এর জন্য আলাদা LoadBalancer বানালে খরচ আকাশছোঁয়া হবে (প্রতিটা ALB-র নিজস্ব বিল)। **Ingress** একটা LB-র পেছনে অনেক service বসানোর উপায়।

```
                  ইন্টারনেট
                      │
              একটাই Load Balancer
                      │
            Ingress Controller (nginx/traefik/ALB controller)
              ┌───────┴────────┐
        /api → api-service   /app → app-service
```

দুইটা জিনিস গুলিয়ে ফেলা যাবে না:

- **Ingress** = শুধু নিয়মের তালিকা, একটা YAML। নিজে কিছু করে না।
- **Ingress Controller** = আসল সফটওয়্যার (nginx ইত্যাদি) যা ওই নিয়ম পড়ে ট্রাফিক পাঠায়। **এটা আলাদা করে ইনস্টল করতে হয়।**

> **খুব কমন beginner সমস্যা:** Ingress বানানো হয়েছে কিন্তু কিছুই হচ্ছে না, `ADDRESS` কলাম খালি। কারণ ক্লাস্টারে কোনো Ingress Controller ইনস্টল করা নেই।

<a id="s7-6"></a>

### ৭.৬ kube-proxy ও NetworkPolicy — **[কাজচালানো]**

**kube-proxy** প্রতিটা node-এ চলে আর Service-এর ভার্চুয়াল IP-কে আসল pod IP-তে পাঠানোর নিয়ম বসায় — iptables mode (default) বা IPVS mode (অনেক service হলে দ্রুত)। ভেতরে কী হচ্ছে সেটা জানা থাকলেই যথেষ্ট, হাতে কনফিগ করা লাগে না।

**NetworkPolicy** — pod-এর মধ্যে firewall। দুইটা জিনিস মনে রাখো:

১. **Default-এ সব pod সব pod-এর সাথে কথা বলতে পারে।** কোনো নিরাপত্তা নেই।
২. একটা pod-এর উপর একটাও NetworkPolicy লাগলেই সে **default-deny** হয়ে যায় — তখন যা যা দরকার সব স্পষ্ট করে লিখতে হবে।

```yaml
# সব inbound বন্ধ করার নিয়ম (তারপর দরকারমতো খুলবে)
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny-ingress
spec:
  podSelector: {}
  policyTypes: [Ingress]
```

> **সবচেয়ে কমন ফাঁদ:** egress deny লিখে DNS (UDP+TCP ৫৩) খুলতে ভুলে যাওয়া। তখন pod কোনো নামই resolve করতে পারে না, আর সমস্যাটা দেখতে সম্পূর্ণ অন্যরকম লাগে।

**সতর্কতা:** NetworkPolicy কাজ করার জন্য CNI-র সাপোর্ট লাগে। Flannel এটা সাপোর্ট করে না — policy লিখলে নীরবে উপেক্ষিত হবে, কোনো ত্রুটিও দেখাবে না।

<a id="s7-7"></a>

### ৭.৭ k8s নেটওয়ার্ক ডিবাগ করার নিয়ম — **[গভীর]**

এই ক্রমটা মুখস্থ করে ফেলো:

```bash
# ১. pod আদৌ চলছে আর Ready?
kubectl get pods -o wide

# ২. Service-এর পেছনে কোনো pod আছে?  ← সবচেয়ে বেশি এখানেই ধরা পড়ে
kubectl get endpoints <service>

# ৩. Service-এর সংজ্ঞা দেখো — selector আর targetPort ঠিক?
kubectl describe svc <service>

# ৪. ভেতর থেকে পরীক্ষা করো (টুলসহ একটা অস্থায়ী pod)
kubectl run tmp --rm -it --image=nicolaka/netshoot -- bash
  # ভেতরে:
  nslookup <service>
  curl -v http://<service>:<port>
  nc -zv <service> <port>

# ৫. DNS চলছে?
kubectl get pods -n kube-system -l k8s-app=kube-dns
kubectl logs -n kube-system -l k8s-app=kube-dns --tail=50

# ৬. NetworkPolicy আটকাচ্ছে?
kubectl get networkpolicy -A

# ৭. Ingress-এর ADDRESS পেয়েছে?
kubectl get ingress
```

**মনে রাখার সূত্র:** `endpoints` খালি → selector/label-এর সমস্যা। `endpoints` আছে কিন্তু curl ব্যর্থ → targetPort ভুল, অ্যাপ ভুল ইন্টারফেসে bind, অথবা NetworkPolicy।

#### 🔧 হাতে-কলমে — ইচ্ছা করে ভাঙো

```bash
kind create cluster --name netlab

kubectl create deployment web --image=nginx --replicas=2
kubectl expose deployment web --port=80
kubectl get endpoints web        # দুইটা IP দেখবে — মনে রাখো

# ভেতর থেকে কাজ করছে কিনা দেখো
kubectl run tmp --rm -it --image=nicolaka/netshoot -- curl -s -o /dev/null -w "%{http_code}\n" http://web

# ─── এবার ভাঙো: selector-এ টাইপো ঢোকাও ───
kubectl patch svc web -p '{"spec":{"selector":{"app":"wbe"}}}'
kubectl get endpoints web        # খালি! এটাই সেই লক্ষণ
kubectl run tmp --rm -it --image=nicolaka/netshoot -- curl -m 5 http://web

# ঠিক করো
kubectl patch svc web -p '{"spec":{"selector":{"app":"web"}}}'
kubectl get endpoints web        # আবার ফিরে এলো

# ─── দ্বিতীয় ভাঙা: ভুল targetPort ───
kubectl patch svc web -p '{"spec":{"ports":[{"port":80,"targetPort":8080}]}}'
kubectl get endpoints web        # endpoints কিন্তু আছে!
kubectl run tmp --rm -it --image=nicolaka/netshoot -- curl -m 5 http://web
# ব্যর্থ হবে — কিন্তু লক্ষণটা আগেরটার চেয়ে আলাদা। পার্থক্যটা লিখে রাখো।

kind delete cluster --name netlab
```

**এই দুইটা ভাঙা-ঠিক করার অভিজ্ঞতা k8s নেটওয়ার্ক ইন্টারভিউয়ের অর্ধেক প্রশ্নের উত্তর দিয়ে দেবে।**

#### 🎤 ইন্টারভিউ প্রশ্ন

<details>
<summary>▸ Pod A থেকে Pod B-তে পৌঁছানো যাচ্ছে না। কীভাবে ডিবাগ করবে?</summary>

স্তরে স্তরে নামব:

১. **pod চলছে?** `kubectl get pods -o wide` — B কি Running আর Ready? CrashLoop-এ না তো?

২. **সরাসরি pod IP-তে কাজ করে?** A-র ভেতর থেকে `curl <podIP>:<port>`। কাজ করলে সমস্যা Service-এ, pod-এ না।

৩. **Service-এর endpoints আছে?** `kubectl get endpoints <svc>` — খালি হলে selector-এর label pod-এর label-এর সাথে মিলছে না, অথবা pod Ready না।

৪. **DNS resolve হচ্ছে?** `nslookup <svc>.<ns>` — না হলে CoreDNS দেখব।

৫. **পোর্ট ঠিক?** Service-এর `targetPort` আর container-এর আসল পোর্ট মিলছে কিনা। অ্যাপ `0.0.0.0`-তে bind হয়েছে কিনা।

৬. **NetworkPolicy আটকাচ্ছে?** `kubectl get networkpolicy -A` — B-র namespace-এ deny আছে কিনা।

ডিবাগের জন্য টুলসহ একটা pod নেব: `kubectl run tmp --rm -it --image=nicolaka/netshoot -- bash` — এর ভেতরে dig, curl, nc, tcpdump সব আছে।

</details>

<details>
<summary>▸ ClusterIP, NodePort আর LoadBalancer — কখন কোনটা?</summary>

**ClusterIP** — ডিফল্ট, আর ভেতরের সব যোগাযোগের জন্য এটাই ব্যবহার করি। backend, database, internal API — যা বাইরে থেকে ডাকার দরকার নেই।

**NodePort** — প্রতিটা node-এ ৩০০০০–৩২৭৬৭ রেঞ্জের একটা পোর্ট খুলে দেয়। লোকাল ডেভেলপমেন্ট বা kind/minikube-এ দ্রুত পরীক্ষা করতে ভালো। প্রোডাকশনে সাধারণত ব্যবহার করি না — পোর্ট নম্বর অদ্ভুত, node-এর IP জানতে হয়, আর node বদলালে ঝামেলা।

**LoadBalancer** — cloud provider-কে বলে একটা আসল LB বানাতে। কাজ করে, কিন্তু **প্রতিটা Service-এর জন্য আলাদা LB মানে আলাদা বিল**। ১০টা service মানে ১০টা LB।

তাই প্রোডাকশনে সাধারণ প্যাটার্ন: **সব service ClusterIP, আর সামনে একটা Ingress** — একটাই LB, তার পেছনে path/host দেখে সব service-এ রাউটিং।

</details>

<details>
<summary>▸ Service কীভাবে জানে কোন pod-এ ট্রাফিক পাঠাতে হবে?</summary>

**Label selector** দিয়ে। Service-এর `spec.selector`-এ যে label লেখা থাকে, সেই label যেসব pod-এ আছে তাদের IP নিয়ে k8s একটা **Endpoints** (নতুন সংস্করণে EndpointSlice) বস্তু বানায়।

তারপর প্রতিটা node-এ **kube-proxy** সেই তালিকা দেখে iptables (বা IPVS) নিয়ম বসায়, যাতে ClusterIP-তে যাওয়া ট্রাফিক কোনো একটা pod IP-তে পৌঁছায়।

গুরুত্বপূর্ণ দিক: **শুধু Ready pod-ই endpoints-এ যোগ হয়**। readinessProbe ব্যর্থ হলে pod চলতে থাকলেও ট্রাফিক পাবে না — এটা ইচ্ছাকৃত, যাতে প্রস্তুত না হওয়া pod-এ ট্রাফিক না যায়।

এজন্যই ডিবাগের প্রথম কমান্ড `kubectl get endpoints <svc>` — খালি মানে হয় selector ভুল, নয়তো কোনো pod Ready না।

</details>

<details>
<summary>▸ Pod CIDR আর VPC CIDR overlap করলে কী হয়?</summary>

রাউটিং ভেঙে যায়। node বুঝতে পারে না `10.0.5.3` বলতে VPC-র একটা instance বোঝাচ্ছে নাকি একটা pod — ফলে ট্রাফিক ভুল জায়গায় যায় বা হারিয়ে যায়। লক্ষণ সাধারণত এলোমেলো: কিছু সংযোগ কাজ করে, কিছু করে না।

বড় সমস্যা হলো **CIDR পরে বদলানো যায় না** — ঠিক করতে হলে ক্লাস্টার নতুন করে বানাতে হয়।

তাই ক্লাস্টার বানানোর আগেই তিনটা রেঞ্জ আলাদা করে লিখে রাখি: VPC/node CIDR, pod CIDR, service CIDR — তিনটার কেউ কারো সাথে, বা অন্য কোনো peered নেটওয়ার্কের সাথে overlap করবে না।

</details>

<details>
<summary>▸ Ingress আর LoadBalancer Service-এর পার্থক্য কী?</summary>

**LoadBalancer Service** L4-এ কাজ করে আর প্রতিটা Service-এর জন্য cloud provider-এর কাছে একটা আলাদা load balancer চায়। সহজ, কিন্তু খরচ service-এর সংখ্যার সাথে বাড়ে।

**Ingress** L7-এ কাজ করে — একটাই load balancer, তার পেছনে host আর path দেখে অনেক service-এ রাউটিং। সাথে TLS termination, redirect, rewrite — এগুলোও এক জায়গায় করা যায়।

একটা জিনিস মনে রাখা জরুরি: Ingress নিজে কিছু করে না, ওটা শুধু নিয়মের তালিকা। কাজটা করে **Ingress Controller** (nginx, traefik, AWS Load Balancer Controller), যা আলাদা করে ইনস্টল করতে হয়। controller ছাড়া Ingress বানালে কিছুই ঘটবে না — `ADDRESS` কলাম খালি পড়ে থাকবে।

</details>

[↑ সূচিপত্রে ফিরে যাও](#index)

---

<a id="s8"></a>

## ৮. Security — **[গভীর]**

Networking-এর নিরাপত্তা আলাদা কোনো বিষয় না — এটা প্রতিদিনের কাজের অংশ। কয়েকটা নিয়ম, যেগুলো না মানলে একদিন না একদিন ভুগতেই হবে।

### নিয়ম ১ — যা বাইরে থাকার দরকার নেই, তা বাইরে রেখো না

```
ইন্টারনেট থেকে খোলা থাকতে পারে   →  80, 443 (LB-তে)
কখনোই খোলা রাখবে না               →  3306, 5432, 6379, 27017, 9200
সীমিত রাখো                         →  22 (শুধু অফিসের IP, বা SSM ব্যবহার করো)
```

Redis আর Elasticsearch বিশেষ করে বিপজ্জনক — অনেক ক্ষেত্রে default-এ কোনো পাসওয়ার্ডই থাকে না। ইন্টারনেটে খোলা থাকলে কয়েক ঘণ্টার মধ্যেই স্ক্যানার পেয়ে যায়।

### নিয়ম ২ — SG-তে IP না, SG রেফার করো

```
❌  App SG: inbound 5432 ← 10.0.11.0/24
✅  DB SG : inbound 5432 ← sg-app
```

দ্বিতীয়টায় কোন জিনিস কার সাথে কথা বলতে পারে সেটা নাম দেখেই বোঝা যায়, আর auto-scaling-এ IP বদলালেও ভাঙে না।

### নিয়ম ৩ — SSH-এর দরজা ছোট রাখো

- পাসওয়ার্ড না, শুধু key
- ২২ পোর্ট `0.0.0.0/0`-তে না — অফিসের IP, বা VPN, বা সবচেয়ে ভালো **AWS SSM Session Manager** (তখন ২২ পোর্ট খোলার দরকারই নেই, bastion-ও লাগে না)
- root দিয়ে সরাসরি লগইন বন্ধ

### নিয়ম ৪ — Certificate-এর মেয়াদে alert বসাও

Cert expiry-তে সাইট ডাউন হওয়া সবচেয়ে বিব্রতকর incident, কারণ এটা ১০০% আগে থেকে জানা যায়। ৩০ দিন আগে alert বসাও, আর সম্ভব হলে স্বয়ংক্রিয় নবায়ন (ACM, cert-manager, certbot) ব্যবহার করো।

### নিয়ম ৫ — নিজের উপর নিজে নজর রাখো

```bash
# তোমার account-এ কোন SG-তে বিশ্বের জন্য দরজা খোলা?
aws ec2 describe-security-groups \
  --query "SecurityGroups[?IpPermissions[?contains(IpRanges[].CidrIp, '0.0.0.0/0')]].{Name:GroupName,Id:GroupId}" \
  --output table
```

#### 🔧 হাতে-কলমে

তোমার AWS account-এ উপরের কমান্ডটা চালাও। যা যা `0.0.0.0/0`-তে খোলা পেলে, প্রতিটার পাশে লিখো — এটা কেন খোলা থাকা দরকার? যেগুলোর উত্তর দিতে পারবে না, সেগুলো বন্ধ করো।

#### 🎤 ইন্টারভিউ প্রশ্ন

<details>
<summary>▸ একটা Security Group-এ ২২ পোর্ট <code>0.0.0.0/0</code>-তে খোলা দেখলে কী করবে?</summary>

প্রথমে বুঝব কী চলছে সেখানে আর কে ব্যবহার করে — না জেনে বন্ধ করলে হয়তো কারো কাজ ভেঙে যাবে, বা কোনো অটোমেশন থেমে যাবে।

তারপর ক্রম অনুযায়ী বিকল্প প্রস্তাব করব:
১. **সবচেয়ে ভালো:** SSM Session Manager — ২২ পোর্ট খোলার দরকারই নেই, bastion-ও না, আর প্রতিটা session-এর অডিট লগ থাকে
২. VPN বা অফিসের নির্দিষ্ট IP রেঞ্জে সীমিত করা
৩. অন্তত একটা bastion host-এর SG-তে সীমিত করা

সাথে পাসওয়ার্ড লগইন বন্ধ আছে কিনা, root লগইন বন্ধ কিনা, আর key কীভাবে ব্যবস্থাপনা হচ্ছে সেটাও দেখব।

আর সবচেয়ে জরুরি — শুধু এই একটা ঠিক করে থেমে যাব না। পুরো account-এ স্ক্যান চালাব, কারণ একটা থাকলে সাধারণত আরও আছে।

</details>

[↑ সূচিপত্রে ফিরে যাও](#index)

---

<a id="s9"></a>

## ৯. আরও উপরে — শুধু নাম জেনে রাখো — **[নাম-জানা]**

এগুলো এখন শেখার দরকার নেই। শুধু জেনে রাখো জিনিসগুলো আছে, যাতে কখনো সামনে পড়লে চিনতে পারো।

| জিনিস | এক লাইনে | কখন সামনে আসবে |
|---|---|---|
| **BGP** | ইন্টারনেটের রাউটিং প্রোটোকল | Calico বা MetalLB কনফিগ করার সময় |
| **VXLAN / overlay** | এক নেটওয়ার্কের ভেতর আরেকটা নেটওয়ার্ক মোড়ানো | k8s CNI-র ভেতরের কাজ, MTU সমস্যা |
| **Proxy Protocol** | L4 LB-র পেছনে আসল client IP পাঠানোর উপায় | NLB-র পেছনে client IP হারালে |
| **keepalived / VRRP** | দুইটা সার্ভারের মধ্যে একটা ভার্চুয়াল IP ভাগাভাগি | on-prem HA সেটআপে |
| **Anycast** | একই IP দুনিয়ার বহু জায়গায় | CDN, DNS (8.8.8.8) |
| **eBPF** | kernel-এ নিরাপদে কোড চালানো | Cilium, আধুনিক observability |
| **Service mesh** | service-এর মধ্যে ট্রাফিক, নিরাপত্তা, পর্যবেক্ষণ | Istio/Linkerd, বড় microservice সেটআপে |
| **QUIC / HTTP3** | UDP-র উপর HTTP | CDN কনফিগে |
| **Zero trust** | নেটওয়ার্কের ভেতরে থাকা মানেই বিশ্বাস না | নিরাপত্তা আলোচনায় |

[↑ সূচিপত্রে ফিরে যাও](#index)

---

<a id="s10"></a>

## ১০. ৩০ দিনের প্ল্যান

দিনে ১–২ ঘণ্টা ধরে সাজানো। প্রতিদিন **পড়া ২০%, হাতে-কলমে ৮০%**।

### সপ্তাহ ১ — ভিত্তি

| দিন | কাজ |
|---|---|
| ১ | ল্যাব সেটআপ (WSL/VM, Docker, টুল ইনস্টল)। [১.১ IP](#s1-1) পড়ো আর bind পরীক্ষাটা করো |
| ২ | [১.২ Subnet/CIDR](#s1-2) — তিনটা সমস্যাই হাতে করো, `ipcalc` দিয়ে মিলাও |
| ৩ | CIDR আরও ১০টা অনুশীলন। কাগজে-কলমে, ক্যালকুলেটর ছাড়া |
| ৪ | [১.৪ TCP state](#s1-4) — `nc` দিয়ে সংযোগ বানাও, `ss` দিয়ে state দেখো |
| ৫ | [১.৫ Port](#s1-5) + refused vs timeout নিজে বানাও |
| ৬ | [২. DNS](#s2) পুরোটা — `dig`-এর সব অপশন চালাও |
| ৭ | **পুনরাবৃত্তি:** কাউকে (বা নিজেকে) সাবটাইটেল ছাড়া বুঝিয়ে বলো — DNS কীভাবে কাজ করে |

### সপ্তাহ ২ — HTTP, TLS, Linux

| দিন | কাজ |
|---|---|
| ৮ | [৩.১–৩.২ HTTP](#s3-1) — status code মুখস্থ, 502 vs 504 বিশেষভাবে |
| ৯ | [৩.৩ Header](#s3-3) — `curl -v` দিয়ে ১০টা সাইট পরীক্ষা করো |
| ১০ | [৩.৪ TLS](#s3-4) — সনদের মেয়াদ বের করো, self-signed বানাও |
| ১১ | [৪.১ Routing](#s4-1) — routing table পড়ো, `ip route get` চালাও |
| ১২ | [৪.২ NAT](#s4-2) + [৪.৩ iptables](#s4-3) — Docker-এর DNAT নিয়ম নিজের চোখে দেখো |
| ১৩ | [৫. Troubleshooting](#s5) — তিনটা ভাঙা সমস্যাই বানাও আর ধরো |
| ১৪ | **পুনরাবৃত্তি:** চার ধাপের runbook মুখস্থ। একজনকে বুঝিয়ে বলো |

### সপ্তাহ ৩ — Cloud

| দিন | কাজ |
|---|---|
| ১৫ | AWS account, billing alarm। [৬.১ VPC](#s6-1) পড়ো |
| ১৬ | হাতে একটা VPC বানাও — subnet, IGW, route table |
| ১৭ | NAT Gateway যোগ করো, private EC2 থেকে `curl ifconfig.me` |
| ১৮ | [৬.৩ SG vs NACL](#s6-3) — ইচ্ছা করে ব্লক করো, ধরো |
| ১৯ | [৬.৪ ALB](#s6-4) বসাও, health check ইচ্ছা করে ভাঙো (503 দেখো) |
| ২০ | [৬.৫ Flow Logs](#s6-5) চালু করো, REJECT খুঁজে বের করো |
| ২১ | **সব মুছে ফেলো** (বিল বাঁচাও)। তারপর Terraform দিয়ে পুরোটা আবার বানাও |

### সপ্তাহ ৪ — Container ও Kubernetes

| দিন | কাজ |
|---|---|
| ২২ | [৭.১ Docker networking](#s7-1) — custom network, নাম দিয়ে খোঁজা |
| ২৩ | `kind` দিয়ে ক্লাস্টার, [৭.২ মডেল](#s7-2) পড়ো, তিনটা CIDR খুঁজে বের করো |
| ২৪ | [৭.৩ Service](#s7-3) — তিন ধরনই বানাও, পার্থক্য দেখো |
| ২৫ | selector আর targetPort ভাঙো — দুইটার লক্ষণের পার্থক্য লিখে রাখো |
| ২৬ | [৭.৪ CoreDNS](#s7-4) — pod থেকে DNS পরীক্ষা, `ndots` দেখো |
| ২৭ | [৭.৫ Ingress](#s7-5) — controller ইনস্টল, দুইটা service-এ path routing |
| ২৮ | [৭.৬ NetworkPolicy](#s7-6) — deny লিখো, DNS ভুলে যাও, তারপর ধরো |
| ২৯ | [১২-এর ৩০টা প্রশ্ন](#s12) — নিজে মুখে উত্তর দাও, রেকর্ড করো |
| ৩০ | **নিজের একটা লেখা:** "যা যা ভেঙেছি আর ঠিক করেছি" — ব্লগ বা README আকারে |

> শেষ দিনের কাজটা বাদ দিও না। নিজের ভাষায় লিখলে ফাঁকগুলো নিজে থেকেই বেরিয়ে আসে। আর ইন্টারভিউতে এটাই তোমার পোর্টফোলিও।

[↑ সূচিপত্রে ফিরে যাও](#index)

---

<a id="s11"></a>

## ১১. চিট শিট

### সবচেয়ে বেশি লাগবে যে কমান্ডগুলো

```bash
# ── DNS ─────────────────────────────────────────────
dig +short example.com                 # দ্রুত উত্তর
dig +trace example.com                 # পুরো পথ
dig @8.8.8.8 example.com               # নির্দিষ্ট resolver
getent hosts example.com               # অ্যাপ যা দেখে (hosts সহ)

# ── সংযোগ ───────────────────────────────────────────
nc -zv host 443                        # পোর্ট খোলা?
ss -tulpn                              # কে কোন পোর্টে শুনছে
ss -tan | grep -c TIME_WAIT            # state গোনা

# ── HTTP / TLS ──────────────────────────────────────
curl -v https://site.com               # সব ধাপ একসাথে
curl -I https://site.com               # শুধু header
curl -k https://site.com               # সনদ বাদ (কাজ করলে সনদের দোষ)
curl -L https://site.com               # redirect অনুসরণ
curl --resolve site.com:443:1.2.3.4 https://site.com   # DNS বাইপাস
echo | openssl s_client -connect site.com:443 -servername site.com 2>/dev/null \
  | openssl x509 -noout -dates         # সনদের মেয়াদ

# ── রাউটিং ──────────────────────────────────────────
ip addr                                # ইন্টারফেস
ip route                               # routing table
ip route get 8.8.8.8                   # কোন পথে যাবে

# ── প্যাকেট ─────────────────────────────────────────
sudo tcpdump -i any port 443 -n        # লাইভ দেখা
mtr example.com                        # প্রতি hop-এ loss

# ── Kubernetes ──────────────────────────────────────
kubectl get endpoints <svc>            # ← সবার আগে এটা
kubectl describe svc <svc>
kubectl run tmp --rm -it --image=nicolaka/netshoot -- bash
kubectl exec -it <pod> -- cat /etc/resolv.conf
```

### পোর্ট

| 22 | 53 | 80 | 443 | 3306 | 5432 | 6379 | 6443 | 27017 |
|---|---|---|---|---|---|---|---|---|
| SSH | DNS | HTTP | HTTPS | MySQL | Postgres | Redis | k8s API | MongoDB |

### CIDR

| /16 | /20 | /24 | /26 | /28 |
|---|---|---|---|---|
| 65,536 | 4,096 | 256 | 64 | 16 |

### Status code

| 401 | 403 | 404 | 429 | 500 | 502 | 503 | 504 |
|---|---|---|---|---|---|---|---|
| কে তুমি জানি না | জানি, অনুমতি নেই | নেই | বেশি হয়ে গেছে | অ্যাপে exception | upstream মরা | healthy backend নেই | upstream ধীর |

### ত্রুটি → কোথায় খুঁজবে

| যা দেখছো | কোথায় খুঁজবে |
|---|---|
| Connection **refused** | service বন্ধ, ভুল পোর্ট, `127.0.0.1`-এ bind |
| Connection **timeout** | Security Group, NACL, firewall, route |
| **NXDOMAIN** | DNS record নেই, ভুল নাম |
| **certificate expired** | সনদ নবায়ন |
| **unable to get local issuer** | intermediate সনদ বাদ পড়েছে |
| **502** | upstream মরা, ভুল পোর্ট, endpoints খালি |
| **503** | কোনো healthy target নেই → health check দেখো |
| **504** | backend ধীর, timeout সেটিং |
| **ERR_TOO_MANY_REDIRECTS** | `X-Forwarded-Proto` অ্যাপ পড়ছে না |
| ছোটটা চলে, বড়টা ঝোলে | MTU |
| k8s-এ endpoints খালি | selector-এ label মিলছে না |

[↑ সূচিপত্রে ফিরে যাও](#index)

---

<a id="s12"></a>

## ১২. দ্রুত ইন্টারভিউ প্রশ্ন — ৩০টা

নিজে মুখে উত্তর দাও, তারপর খুলে মিলাও। উত্তর পারলেও **উচ্চারণ করে বলা** অভ্যাস করো — জানা আর বলতে পারা এক জিনিস না।

<details>
<summary>▸ ১. ব্রাউজারে URL দিলে কী কী ঘটে?</summary>

DNS resolve → TCP handshake → TLS handshake → HTTP request → উত্তর। প্রতিটা ধাপ [৩.৫-এর বিস্তারিত উত্তরে](#s3-5) আছে।

</details>

<details>
<summary>▸ ২. TCP আর UDP-র পার্থক্য?</summary>

TCP সংযোগভিত্তিক, নির্ভরযোগ্য, ক্রম বজায় রাখে — HTTP, SSH, database। UDP সংযোগহীন, দ্রুত, নিশ্চয়তা নেই — DNS, ভিডিও, গেম, VPN।

</details>

<details>
<summary>▸ ৩. Connection refused vs timeout?</summary>

Refused = প্যাকেট পৌঁছেছে, কেউ শুনছে না (RST ফেরত এসেছে) → অ্যাপের সমস্যা। Timeout = কোনো উত্তরই নেই, নীরবে drop → firewall/SG/route-এর সমস্যা।

</details>

<details>
<summary>▸ ৪. <code>/24</code> subnet-এ কয়টা host?</summary>

256টা মোট, সাধারণ নেটওয়ার্কে 254টা ব্যবহারযোগ্য, AWS-এ 251টা (৫টা সংরক্ষিত)।

</details>

<details>
<summary>▸ ৫. A record আর CNAME-এর পার্থক্য?</summary>

A → IP, CNAME → আরেকটা নাম। Root domain-এ CNAME দেওয়া যায় না (Route 53-এ ALIAS ব্যবহার করতে হয়)।

</details>

<details>
<summary>▸ ৬. TTL কেন গুরুত্বপূর্ণ?</summary>

উত্তর কতক্ষণ cache থাকবে তা ঠিক করে। Migration-এর আগে TTL কমিয়ে না রাখলে DNS বদলানোর পরেও দীর্ঘ সময় পুরোনো সার্ভারে ট্রাফিক যেতে থাকবে।

</details>

<details>
<summary>▸ ৭. 502 আর 504?</summary>

502 = upstream মরা বা ভাঙা উত্তর। 504 = upstream আছে কিন্তু সময়মতো উত্তর দেয়নি।

</details>

<details>
<summary>▸ ৮. 401 আর 403?</summary>

401 = তুমি কে জানি না (টোকেন নেই/ভুল)। 403 = জানি, কিন্তু অনুমতি নেই।

</details>

<details>
<summary>▸ ৯. TLS handshake-এ কী হয়?</summary>

ClientHello (সমর্থিত cipher + SNI) → ServerHello + certificate → সনদ যাচাই (CA চেইন, নাম, মেয়াদ) → key exchange → এনক্রিপ্টেড সংযোগ।

</details>

<details>
<summary>▸ ১০. SNI কী?</summary>

TLS handshake-এর শুরুতেই client কোন hostname চাইছে তা জানায়, যাতে একই IP-তে বহু সাইট থাকলে সার্ভার ঠিক সনদটা পাঠাতে পারে।

</details>

<details>
<summary>▸ ১১. "ব্রাউজারে কাজ করে কিন্তু curl-এ সনদের ত্রুটি" — কেন?</summary>

প্রায় সবসময় **intermediate certificate বাদ পড়েছে**। ব্রাউজার অনেক সময় নিজে খুঁজে নেয়, curl নেয় না। সমাধান: fullchain বসাও।

</details>

<details>
<summary>▸ ১২. Security Group আর NACL?</summary>

SG instance-এ, stateful, শুধু allow। NACL subnet-এ, stateless, allow+deny। NACL-এ উত্তরের জন্য ephemeral port আলাদা করে খুলতে হয়।

</details>

<details>
<summary>▸ ১৩. Public আর private subnet-এর পার্থক্য?</summary>

শুধু route table। `0.0.0.0/0 → IGW` থাকলে public, `0.0.0.0/0 → NAT` থাকলে private।

</details>

<details>
<summary>▸ ১৪. Private subnet থেকে ইন্টারনেটে কীভাবে যায়?</summary>

NAT Gateway দিয়ে, যা নিজে public subnet-এ বসে। একমুখী — বাইরে থেকে ভেতরে আসা যায় না।

</details>

<details>
<summary>▸ ১৫. NAT Gateway public subnet-এ কেন?</summary>

তার নিজের route table-এ IGW দরকার, নইলে সে নিজেই বাইরে যেতে পারবে না।

</details>

<details>
<summary>▸ ১৬. VPC peering transitive?</summary>

না। প্রতিটা জোড়ার জন্য আলাদা peering + route লাগে। বেশি VPC হলে Transit Gateway।

</details>

<details>
<summary>▸ ১৭. ALB আর NLB?</summary>

ALB L7 (path/host routing, HTTP বোঝে), NLB L4 (দ্রুত, স্থির IP, client IP সংরক্ষণ)।

</details>

<details>
<summary>▸ ১৮. ALB থেকে 503 — কেন?</summary>

কোনো healthy target নেই। health check-এর path/port, target-এর SG, আর অ্যাপের bind address দেখো।

</details>

<details>
<summary>▸ ১৯. VPC Flow Logs কী কাজে?</summary>

কোন ট্রাফিক ACCEPT আর কোনটা REJECT হলো তার রেকর্ড। Cloud-এ "timeout কেন" ধরার সবচেয়ে সরাসরি উপায়।

</details>

<details>
<summary>▸ ২০. <code>docker run -p 8080:80</code> কী করে?</summary>

iptables-এর NAT টেবিলে একটা DNAT নিয়ম বসায়। container-এর অ্যাপ `0.0.0.0`-তে bind না হলে কাজ করবে না।

</details>

<details>
<summary>▸ ২১. docker-compose-এ service-রা নাম দিয়ে কীভাবে একে অপরকে পায়?</summary>

compose একটা custom bridge network বানায়, আর custom network-এ Docker-এর নিজস্ব DNS চালু থাকে — তাই container-এর নামই hostname হিসেবে কাজ করে।

</details>

<details>
<summary>▸ ২২. Kubernetes-এর নেটওয়ার্ক মডেলের মূল নিয়ম?</summary>

প্রতিটা pod-এর নিজের IP, আর যেকোনো pod যেকোনো pod-এর সাথে NAT ছাড়া সরাসরি কথা বলতে পারে — অন্য node-এ হলেও। CNI plugin এটা বাস্তবায়ন করে।

</details>

<details>
<summary>▸ ২৩. ClusterIP, NodePort, LoadBalancer?</summary>

ClusterIP ভেতরের জন্য, NodePort node-এর পোর্ট খোলে (dev-এ), LoadBalancer cloud LB বানায় (প্রতিটার আলাদা বিল)। প্রোডাকশনে সাধারণত ClusterIP + Ingress।

</details>

<details>
<summary>▸ ২৪. Service pod খুঁজে পায় কীভাবে?</summary>

Label selector → Endpoints/EndpointSlice → kube-proxy-র iptables নিয়ম। শুধু **Ready** pod endpoints-এ যায়।

</details>

<details>
<summary>▸ ২৫. "Service কাজ করছে না" — প্রথম কমান্ড কী?</summary>

`kubectl get endpoints <svc>`। খালি মানে selector মিলছে না বা pod Ready না।

</details>

<details>
<summary>▸ ২৬. CoreDNS-এ Service-এর পুরো নাম?</summary>

`<service>.<namespace>.svc.cluster.local`। search domain-এর কারণে একই namespace-এ শুধু `<service>` লিখলেই চলে।

</details>

<details>
<summary>▸ ২৭. k8s-এ DNS ধীর — কেন হতে পারে?</summary>

`ndots:5` — external নাম resolve করতে গিয়ে আগে সব search domain চেষ্টা হয়। FQDN-এ শেষে ডট দাও, বা `dnsConfig`-এ ndots কমাও, বা NodeLocal DNSCache।

</details>

<details>
<summary>▸ ২৮. Ingress আর LoadBalancer Service?</summary>

Ingress = একটা LB-তে বহু service (L7 routing), কিন্তু Ingress Controller আলাদা করে ইনস্টল করতে হয়। LoadBalancer Service = প্রতিটার জন্য আলাদা LB।

</details>

<details>
<summary>▸ ২৯. NetworkPolicy-র default আচরণ?</summary>

Default-এ সব pod সবার সাথে কথা বলতে পারে। একটা policy লাগলেই সেই pod default-deny হয়ে যায়। Egress deny লিখলে DNS (৫৩, UDP+TCP) খুলতে ভুলো না। CNI সাপোর্ট না করলে (যেমন Flannel) policy নীরবে উপেক্ষিত হয়।

</details>

<details>
<summary>▸ ৩০. "সাইট ডাউন" — কীভাবে এগোবে?</summary>

আগে দুইটা প্রশ্ন: সবার জন্য না কিছু জনের? কখন থেকে, আগে কী বদলেছে? তারপর চার ধাপ: DNS → TCP → TLS → HTTP। প্রতিটা আলাদা করে যাচাই, অনুমান না। আর আগে সমস্যা থামাও (রোলব্যাক), পরে কারণ খোঁজো।

</details>

[↑ সূচিপত্রে ফিরে যাও](#index)

---

## শেষ কথা

এই নোটে যা আছে তা পড়ে শেষ করা যায় এক সপ্তাহে। কিন্তু **কাজে লাগানোর মতো করে শিখতে ৩০ দিন লাগবে**, কারণ হাতে-কলমে অংশগুলোই আসল।

তিনটা কথা মনে রেখো:

১. **চার ধাপের runbook** ([৫.১](#s5-1)) — এই একটা জিনিস তোমাকে বেশিরভাগ সমস্যায় পথ দেখাবে
২. **ভেঙে ঠিক করা** — পড়ে শেখার চেয়ে দশগুণ ভালো মনে থাকে, আর ইন্টারভিউতে বলার গল্প তৈরি হয়
৩. **networking একা যথেষ্ট না** — Linux, Git, CI/CD, Docker, Kubernetes, Terraform, observability — এগুলোও লাগবে। কিন্তু networking শক্ত থাকলে বাকিগুলোর সমস্যা ধরা অনেক সহজ হয়ে যাবে

শুভকামনা। 🚀

[↑ সূচিপত্রে ফিরে যাও](#index)
