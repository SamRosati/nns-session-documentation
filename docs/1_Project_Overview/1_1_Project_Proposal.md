# 1.1 Project Proposal

## Problem Statement

In the beach volleyball community in Vancouver, there are several different organizations spanning from **very casual** (group chats with a few friends) to **very formal** (official provincial organization). The official organizations use legacy software for registrations for their tournaments, however the system has poor UI / UX design, and it doesn't solve any of the actual tournament-day automation.

Although solving the registration aspect for tournaments is valuable, there is much more that can be done to make running tournaments less frustrating and free up time for organizers to focus on challenges that can't be automated. For example, automating the process of seeding players that register for a given tournament, creating the seeding round match ups, creating the playoff brackets, results tracking, and providing a way for players to move from one tournament to another with a standardized point collection system is something that existing solutions lack.

## Proposed Solution

The solution I want to create will be a software platform that organizes data in the volleyball community, and connects athletes with organizers and coaches. Initially, the main focus of the app will be to develop a tool for organizers that makes running tournaments much easier and saves organizers time. It will take care of registrations, pool creation, seeding game creation, provide a way for players to input scores on their phones so that organizers don't need to chase players around the beach after each game, create playoff brackets, and track the data associated with every player's wins and losses to provide statistics for each player.

Once the tournament automation aspect of the platform is functional, reliable, and deemed useful by the community, further features will be developed to connect athletes with each other as well as with coaches. Statistics tracking for competitive players who want to carry their points between different tournament series and compete for semi-professional or professional-level tournament entries will be provided with a standardized point system which will allow them to be recognized by organizations such as VolleyballBC, or the FIVB.

After a significant number of athletes are on the platform, features for coaches will be implemented in order to provide them with access to a user base of enthusiastic volleyball players. Coaches will be able to post training clinic events on the athletes feed, and run their coaching business through a dashboard viewable only by coach user accounts. Coaches will be able to be associated with organizations, or can run their business independently, earning ratings from athletes that have taken their clinics.

## 7 Usability Factors

Since I am creating something from scratch rather than redesigning an existing solution, I can evaluate the 7 usability factors of the current alternatives to the platform I want to create.

SportLoMo is designed to manage sports, and it is what VolleyballBC currently uses to manage the leagues and tournaments that they run.

- **Useful**: SportLoMo is useful for registrations, but cannot handle the intricacies of the sport of volleyball because it is a sport management software for all sports, meaning it tries to do everything, and doesn’t solve any one particular sport (like volleyball) to a sufficient enough degree.
- **Usable**: SportLoMo is usable, but the User Interface could be improved, especially for mobile.
- **Findable**: One of the frustrations with using SportLoMo is that searching for the league or tournament that you want to register for is very difficult. There is no search functionality, so you have to scroll through pages until you find what you want.
- **Credible**: SportLoMo has some credibility, since organizations like VolleyballBC use it, however I would not call it extremely credible, given its 3.5/5 star rating on google.
  ![SportLoMo Google Rating](./assets/SportLoMoGoogleRating.png)
- **Accessible**: I believe SportLoMo’s accessibility could improve, although there is nothing inherently inaccessible.
- **Valuable**: The provides some value in that it takes care of registrations, however its attempt to solve all sports rather than just picking one that it serves well makes it less valuable than it could be in my eyes.

## Market Research

I have selected **SportLoMo** (as the local incumbent used by VolleyballBC) and **Volleyball Life** (as the closest direct feature competitor for a modern beach volleyball platform)

## SportLoMo

SportLoMo is a large, enterprise-grade sports management platform primarily designed for national and provincial governing bodies (like Volleyball Canada and Volleyball BC). It excels at “official” administration, handling complex membership tiers, insurance compliance, referee assigning, and linking provincial data to national databases. Because it is the “official” choice, its biggest strength is its ecosystem lock-in. If you want to play in a Volleyball BC tournament, you must use it, guaranteeing a user base.

Its weaknesses however are significant, particularly in User Experience. Users frequently report that the interface is not intuitive, dated, and clunky. It feels more bureaucratic, rather than social. The mobile app has very low ratings due to bugs and poor usability. It is built as a database tool for administrators first and a players app second, meaning it lacks the social glue and easy discovery of events that an app like NNS Session could offer.

## Volleyball Life

Volleyball Life is likely the strongest functional competitor in the beach volleyball space. Unlike generic team apps, it is build specifically for beach volleyball culture. It’s “TruVolley” rating system tracks individual player rankings, rather than just team wins, using a modified Glicko algorithm. It also leans heavily into the ”career” aspect of the sport, offering robust recruiting tools for juniors to connect with college coaches and public player profiles that feel like a social media resume.

The downside lie in its complexity and cost model. It pushes a premium subscription for advanced features like recruiting profiles and video highlights, which feel aggressive for casual adult players who just want to find a game. Additionally, while it is dominant in the US, it is not used in the Canadian social scene.

## Takeaways

After discovering Volleyball Life, I became worried. Discovering what this platform is advertized as being able to do is scary as someone who is trying to develop something very similar, and I did not think that there was already something like that our there.

That being said, I have a pretty biased way of dealing with this problem, which is to continue building my idea for the following reasons:

- Even if this is unsucessful from a business prospective, it will still be a very valuable portfolio piece.

- I believe that any product can be improved. If something already exists in the space I need to build, I just need to make my version better.

In order to address this problem and discover what the gaps are in the market for a volleyball software platform, I used an LLM to reference my codebase, and asked it this question.
