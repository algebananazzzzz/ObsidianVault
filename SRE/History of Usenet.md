### Propagation

1. **Uploader uploads content to one backbone**
    - File is split into many **articles**
    - Each article gets a **unique Message-ID**
    - Articles are posted to one or more **newsgroups**
2. **Announcement**
    - Backbone announces _Message-IDs_ (not data) to peers
3. **Pull-based peering**
    - Other backbones decide whether to pull
    - If pulled, articles are duplicated
    - If skipped, they never appear there
### Downloading

1. **User searches an indexer**
    - Indexer returns an **NZB**
    - NZB = list of Message-IDs + groups
2. **Article retrieval**
	- Client may be configured with multiple **providers** mapped to **one backbone** each
    - For each Message-ID:
        1. Client sends NNTP `BODY <message-id>` to provider A
        2. If found (`222`) → download
        3. If missing (`430`) → try next provider (by priority)
3. **Reconstruction**
    - Client reorders segments, decodes (yEnc), repairs (PAR2)

## UUCP vs NNTP

| **Unix-to-Unix Copy Protocol**                                                                                                  | **Network News Transfer Protocol**                                                                                                              |
| ------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| A **batch-oriented**, **store-and-forward** file transfer system designed for **intermittently connected** Unix machines.       | A **session-based**, r**equest/response protocol** over **TCP** for **selectively advertizing and transferring individual immutable articles**. |
| **Dial-up lines**                                                                                                               | **TCP/IP**                                                                                                                                      |
| **Batch-coupling problem:**<br>- Articles became huge → big batches<br>- One failure blocked everything<br>- No selective retry | - Binaries became feasible on top of files<br>- Shifted toward **machine-driven distribution**                                                  |

## History of Usenet

### 1. Early Usenet: forums that worked

Usenet began as a **heaven** used mainly by universities and **technically literate** users. Traffic was small, articles were plain text, and social norms were strong enough to keep discussions readable. Slow propagation and limited access acted as natural rate limits, so moderation was mostly unnecessary and conversations stayed coherent.

Every September, university freshmen will join Usenet in hordes and make a nuisance of themselves due to their incompetence. Usually, there will be enough **"oldfags"** around to keep the **"newfags"** in-line with gentle trolling. By the end of the month, everything goes back to normal when the new freshmen learnt to behave themselves.
### 2. Eternal September (1993)

That particular September never ended.

In 1993, America Online (dial-up ISP) decided to give its entire userbase free Usenet access. Usenet suddenly finds itself invaded by millions of ignorant **fags** who know nothing of the culture they're trampling over. The **signal-to-noise ratio** went incredibly lopsided, permanently deafening everyone and causing anybody with self-respect to **GTFO**.

### 3. Binary posting shifts Usenet’s purpose

At roughly the same time, NNTP made large-scale binary distribution practical. Binary groups grew rapidly and soon dominated Usenet traffic. This changed incentives: machines downloading files were unaffected by spam or poor discussion quality, while humans were. Usenet gradually shifted from a conversation medium into a bulk content distribution system.

### 4. The web replaced discussion Usenet

As binary traffic grew, universities and commercial ISPs gradually **stopped supporting** Usenet, not only because of rising storage and bandwidth costs, but more importantly due to increasing legal and liability risks. Meanwhile, web-based bulletin boards like BBS offered identity, moderation, and better user experience. Human discussion migrated away from Usenet to centralized web platforms—eventually to sites like **Reddit**.

### 5. The Usenet of Today

Today’s Usenet primarily involves serving large, append-only binary data over NNTP. Such infrastructure requirements are easily met by Usenet providers with large disks and high-capacity bandwidth, keeping costs low. Because providers act as neutral storage and transport services rather than interactive platforms, they also avoid the operational complexity and legal overhead.
