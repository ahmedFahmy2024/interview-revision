# CIB Bank — Behavioral Interview Questions & Answers
**Senior Frontend Developer (React JS) — 21 July 2026, 6:00 PM, Microsoft Teams**

---

## 5.1 "Introduce yourself"

Hi, I'm Ahmed. I have over 3 years of professional experience building web applications with React, Next.js, and TypeScript.

Currently, I'm working at SahlSolution, where I develop enterprise-scale applications like Zad Al-Akhera, a production PWA serving more than 3,000 daily users. I worked on features such as a drag-and-drop quiz builder, an RBAC permission system, offline-first functionality, and complex admin dashboards.

Before that, I worked at Catchy Tech, where I built a real estate auction platform with real-time WebSocket bidding and a geolocation-based marketplace with live messaging.

Although my degree is in Civil Engineering, I transitioned into software because I genuinely enjoy solving technical problems. That background taught me to think carefully about system structure, scalability, and edge cases before implementing solutions.

My core stack is React, Next.js, TypeScript, TanStack Query, and Zustand. I enjoy building maintainable, scalable frontend architectures and collaborating closely with backend engineers and product teams. That's one of the reasons this opportunity stood out to me, as it aligns well with my experience building enterprise applications and internal business systems.

---

## 5.2 "Why are you leaving your current company?"

I've genuinely enjoyed my time at SahlSolution. Working on production applications with thousands of daily users has given me a lot of ownership and hands-on experience, and I've learned a great deal there.

At this point in my career, I'm looking for a new challenge in a larger and more structured engineering environment where I can collaborate with experienced teams, learn from senior engineers, and work on systems with higher requirements around scalability, security, and engineering practices.

That's one of the reasons this opportunity really interests me. I believe it will help me continue growing both technically and professionally.

### "Why did you choose us / CIB?"

From what I've seen, A1Softech works on enterprise solutions across different industries, including banking, where software quality, maintainability, and security are critical. That's the kind of environment I want to grow in. I'm looking forward to working on larger-scale systems, collaborating with experienced engineers, and learning the engineering practices that come with enterprise development.

### "Why leave now?"

I don't have any issues with my current company. I simply feel I've reached a point where I'm ready for the next challenge, and this opportunity aligns well with the direction I want for my career.

---

## 5.4 "What kind of projects do you work on — SaaS, B2B, LMS, etc.?"

Most of my experience has been building B2B internal platforms rather than consumer-facing applications.

For example, I worked on Saldwish, which is an internal admin system for a multi-branch restaurant business integrated with the Foodics POS platform. I also worked on Smart Wash, an operations dashboard for an on-demand car wash service, and UPL, a logistics platform used to manage shipments, drivers, and fleet partners.

Across these projects, I followed the same architectural approach: building reusable, config-driven tables and forms instead of creating every CRUD screen from scratch. That made the applications easier to maintain and scale as new modules were added.

Besides that, I've also worked on LMS platforms like Zad Al-Akhera and OD Practitioner, where I built features such as authentication, RBAC, course and quiz management, and progress tracking. I've also had experience with consumer-facing products, including e-commerce platforms and a real-time auction marketplace.

If I had to describe where I'm strongest, I'd say it's building scalable internal business applications with complex permission models and maintainable frontend architecture. I believe that experience translates very well to enterprise environments like banking.

---

## 5.5 "If you have a tight deadline, how would you behave?"

When I'm working under a tight deadline, the first thing I do is clarify priorities. I identify what's essential for the deadline and what's nice to have, then align with the product owner or team lead as early as possible. I'd rather adjust the scope on day one than miss the deadline unexpectedly.

Next, I break the work into small, deliverable milestones and start with the highest-risk part first. The biggest risks usually come from uncertainty, so I prefer to validate those early instead of leaving them until the end.

If we need to make trade-offs, I focus on postponing lower-priority improvements like extra polish or non-essential refactoring. However, I avoid compromising on things like validation, authentication, authorization, or anything that could affect security or data integrity.

Finally, I keep the team updated throughout the process. If I see a risk, I communicate it early instead of waiting until the deadline. In my experience, early communication gives the team time to adjust priorities or offer help before a small issue becomes a missed delivery.

### "Can you give me a real example?"

On one project, we had a very tight release window. Instead of trying to finish every enhancement, we agreed to deliver the core functionality first and postpone a few UI refinements to the following sprint. That allowed us to release on time without compromising stability or security, and we completed the remaining improvements afterward.

---

## 5.6 "What are your strengths?"

I'd highlight two strengths.

First, ownership. I naturally take responsibility for the features I work on. Across several projects, I've owned major frontend functionality — from authentication and RBAC to complex dashboards and PWA features. I don't just focus on making something work; I also think about maintainability, scalability, and how the code will evolve over time.

Second, I believe I'm good at being honest about technical problems, including my own. I regularly review my work and identify areas that could be improved, whether that's reducing technical debt, simplifying the architecture, or redesigning a solution after learning a better approach. I think that mindset is especially valuable in enterprise environments, where reliability and long-term maintainability matter as much as delivering features.

---

## 5.7 "What are your weaknesses?"

I'd mention two areas I'm actively working on.

The first is automated testing. Most of my recent projects were fast-paced production applications where the priority was delivering features quickly, so although I'm comfortable with Jest and React Testing Library, I haven't worked in a codebase with extensive automated test coverage. That's something I've been investing more time in because I know it's essential in larger engineering teams.

The second is that I sometimes spend extra time refining a technical solution because I enjoy clean architecture and maintainable code. Over time I've learned to balance that with business priorities by aligning early on what's essential for the current release and what can be improved later.

---

## 5.8 "Tell me about a problem you had."

One issue I remember clearly happened in the UPL Dashboard.

We started receiving reports that users could log in successfully, but after authentication they couldn't access the dashboard. There was no visible error, which made the issue difficult to reproduce.

I began by tracing the authentication flow step by step. The backend was returning a valid response, so I inspected the session created by NextAuth. That's when I discovered the JWT contained the entire permissions array, which caused the session cookie to exceed the browser's 4 KB size limit. The cookie write appeared successful, but reading it back became unreliable because of its size.

Instead of trying to compress the data, I redesigned the flow. I kept only the essential authentication data inside the session token and moved permissions to a separate API request after login. That kept the session lightweight while still allowing the UI to fetch permissions when needed.

After the change, authentication became stable again, and the solution was easier to maintain as the permission model evolved. I also documented the root cause so the same issue wouldn't be introduced again in future changes.

---

## 5.9 "How do you deal with pressure?"

I usually think about pressure in two parts: solving the technical problem and keeping communication clear.

On the technical side, I avoid jumping to conclusions. My first step is always to understand the root cause before taking action. For example, during a production issue on one of our platforms, users started experiencing server errors. Instead of immediately restarting services, I investigated the issue, identified the actual cause, and fixed the underlying problem rather than applying a temporary workaround.

On the communication side, I believe visibility is just as important as problem-solving. If something is taking longer than expected, I'd rather keep the team informed with regular updates than go silent. In my experience, early communication helps everyone make better decisions and reduces unnecessary pressure on the team.

### "Have you ever missed a deadline because of pressure?"

Yes, I've had situations where a task took longer than I initially estimated because of unexpected technical issues. When that happened, I communicated the risk early, explained the reason, and worked with the team to adjust priorities instead of waiting until the deadline to report the delay. I learned that managing expectations is just as important as solving the technical problem itself.

---

## 5.10 "If I disagree with a team member?"

If I disagree with a team member, the first thing I do is understand their perspective before defending mine. In my experience, most disagreements happen because people are optimizing for different constraints, not because someone is right and someone is wrong.

After that, I try to make the discussion objective. Instead of saying "I prefer this," I bring concrete examples, edge cases, performance implications, or maintenance trade-offs. If we're still unsure, I'd rather validate the options with a small prototype or involve another experienced teammate than turn it into a personal debate.

For example, I once had a discussion with a backend developer about the structure of an API response. The original response worked, but it forced the frontend to make additional requests and perform unnecessary data transformations. I explained the impact on the frontend, showed how a slightly different response shape would simplify the implementation without adding significant backend complexity, and we discussed the trade-offs together. We eventually agreed on a revised API contract that reduced complexity on both sides. That experience reinforced the importance of solving problems collaboratively rather than trying to "win" the argument.

---

## 5.11 "Where do you see yourself in a year?"

A year from now, I'd like to be someone the team trusts with the more complex, less-defined problems — not just implementing features, but helping shape how they're designed and built.

I'd also like to have strengthened the areas I'm currently improving, especially automated testing, and become fully comfortable with the team's engineering standards, development processes, and codebase.

Most importantly, I'd like to have earned the team's trust by consistently delivering reliable work and taking responsibility for important parts of the product. If, after a year, people see me as someone they can depend on for challenging features and technical discussions, I'd consider that a successful first year.

---

## 5.12 "Why should we choose you?"

I believe I can add value from day one because my experience aligns well with the kind of enterprise frontend applications your teams build.

Over the past few years, I've built production systems with React, Next.js, and TypeScript, including RBAC-based admin platforms, complex dashboards, and large-scale business applications. Along the way, I've learned how to solve real production problems — not just build features — from authentication and permissions to performance and debugging.

Beyond the technical side, I take ownership of my work, I communicate openly with my team, and I genuinely enjoy learning from experienced engineers. I believe I have a solid technical foundation while still being eager to grow, and that's why I think I'd be a good fit for this role.
