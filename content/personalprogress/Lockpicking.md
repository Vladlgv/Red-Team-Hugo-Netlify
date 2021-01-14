---
title: "Lock picking"
date: 2020-12-14T17:06:01+01:00
draft: false
summary: "The goal of lock picking is as simple as to open a lock."
---

### What is Lock picking?


"Lock picking is the practice of unlocking a lock by manipulating the components of the lock device without the original key."
[Wikipedia](https://en.wikipedia.org/wiki/Lock_picking)


The goal of lock picking is as simple as to open a lock. Another more important question would be
#####  __Why would this skill be useful for a penetration tester__ ?

---------
The answer to that question is a bit more complex. Red teaming as a discipline is composed of three major areas digital penetration, social engineering/penetration, and hardware penetration. The goal of this minor is to teach us how to become better professionals in our chosen discipline. Therefore, on top of practicing the hardest but most marketable of the three areas of digital discipline I chose to also practice physical penetration.

The advantage of being able to access physical parts of the target infrastructure is quite massive. Being able to access the routers, switches, servers or personal computers of a target is by itself a vulnerability. From this point on one can plant bugs, reset or delete information directly from the hard drive, disrupt the functions of the network, or simply monitor the target.

As a result of this knowledge, this part of the infrastructure is usually locked away in a room secured by a keycard or by a lock. More often than not companies don't take the time in investing in quality locks, all it takes is the most minimal of lock picking skill and the tools for the craft.

Therefore learning how to lock pick could one day make the difference between a successful penetration test that will lead a company to improve their security or an unbelievable report that the company will discard as a purely theoretical threat leaving their systems vulnerable and the services provided lackluster.

### How to learn how to lock pick?

The first step of the process is to find a lock picking set. The good news was that buying a lock picking set was unnecessary as the Fontys ISSD already had 2 kits. The bad news was that the lock picking set was more popular than anticipated and it took 3 weeks until borrowing the kit at ISSD was possible.

After getting a set the next important part is understanding what all the picks are used for and how to use/ train with them. For this, I have consulted multiple sources but out of all the websites that I looked through the [art-of-lockpicking](https://www.art-of-lockpicking.com/types-of-lock-picks-guide/) had the best guide that explained the different types of lock picks.

In addition to the website I also found some good video material that was just as useful and seeing it done on camera gave me a better idea of how to train with the lock picking set.

{{<youtube gTZddvAws9M>}}

-------

The next steps were to practice the techniques learned in the website and in the video that I saw.

In the base kit given by Fontys there were 3 locks. For my personal goal I set out to be able to unlock all locks using all the techniques available and to open the lock on my own room.

The techniques that I practiced and that were the most useful ordered in their success rate and ease of use were:

1. Raking/Rocking - An erratic and volatile sytle of lock picking where the pins are rocked with an up and down motion to bounce the pins to the shear line.
2. Raking/Scrubbing -using a scrubbing motion with the goal of bouncing the pins to the shear line. 
3. Zipping - violently bounce pins to the shear line by focefully pulling the pick out of th keyway while applying an upward force on the pins
4. Single pin picking - picking a single pin at at time

#### First lock (easiest)
After informing myself from different sources on the internet (amazon and other kit buying vendors). I realised that this is the most common and usually the lock that comes with most lock picking kits.

Knowing this gave me an idea on which one of the most common lock picking techniques to use.

After some experimenting I found that racking with the city rake pick in a combination of rocking and scrubbing was the fastest and most eficient way to open the lock. Zipping was also tried with multiple picks but this did not yield any useful results.

After picking it with the racking technique I practiced opening it using the single pin technique, this proved to be much harder but in the end it was also possible to open the lock in this manner.

The most valuable trick that was learned while trying the various techniques was how to apply pressure with the tension tool. The trick is to apply increasingly more pressure (gently) to the lock as you are going in with the pick for racking .For single pin picking this is even more tricky as the slight ease felt when you put a pin in the sheer line is the only indication given. In order to feel this a lot of practice is needed, in my case only in my second week of trying did I get the hang of this skill.



![LockPick](/Lockpick/scrapeTool.jpg)
![LockPick](/Lockpick/hookTool.jpg)
 ##### [Figure 1.1 - Tools used]

{{<youtube vWy_onA0K4E >}}
 ##### [Video 1 - Breaking into the lock with a racker]

 Video 1 showcases how I opened the first lock using the city rake lock pick. As it can be seen in the video opening the easy lock takes a matter of seconds using the scraping technique.


#### Second lock (medium)

This lock proved to be more difficult in the begining as I did not understand how it functioned. In reality it was quite a simple lock but what I needed to do was find the right technique to use on it.

Like the previous lock I tried scraping and rocking but that did not achieve anything. I also tried single pin picking but each set pin was put too close to the sheer line and as I advanced through them I was forced to reset them because they would go above the sheer line. 

Finally, after a bit of head scratching the realization came that zipping the lock was not tried. This proved to be te solution to this lock. And after just 4 tries it opened. Because with the previous lock the technique did not work it was tossed aside as a useful technique.

As such, the lesson was that not all locks are the same and different locks may require a different technique.

{{<youtube _JsqHEr9460>}}
 ##### [Video 2 - Breaking into the lock using the ziping method]
For the second lock I used a ball end pick lock that was ideal for zipping the lock.
In addition to that the lock was a bit more difficult as it had 2 different ends that at first seemed to have different solutions. However, using different techniques did not prove any more useful.


#### Third lock (hard)

Out of the three practice locks the third lock was the most difficult one. That is because there was no particular trick to it. The only way to open it was with the single pin picking method. I had to move each and every one pin in place without any tricks or fast method techniques. 

Furthermore, because it was such a hard lock to pick opening it was harder to practice. In the end, when a satisfactory result was achieved, it took 15 minutes to open the lock. Probably with more practice and a more experienced lock picker this lock could be opened in 5 minutes or maybe 2. 

The lesson learned from this lock is that lock picking is a skill set after all and the more practice you put into the harder but universal techniques the easier it will be to pick locks.

![LockPick](/Lockpick/3rdlockProof.jpg)
 ##### [Figure 1.2 - proof for opening the 3rd lock]
 I tried to open the 3rd lock on video but it took too much so I could not film the whole process. It took anywhere from 5 minutes to 20 minutes to open this lock. The problem is that with this one there are no easy ways to open it. One needs to pick each pin individually which requires more skill and finess.

{{<youtube WbSXNJU4b1s>}}
##### [Video 3 - Attempt to break into the hard lock on screen]

#### Room Lock (Hardest)

The final test that I subjected the skills that were learned was opening the lock to my own room in my student house. I opened it from the inside out because I did not want to alert my hose mates. This proved to be a bit more difficult because this meant that the pressure tool needed to stay on the left hand or the opposite side.

Sadly, I was not able to get video proof of the moment of opening and I was not able to pick the lock more than once because of the difficulty of the lock but what worked for me was again the single pin picking method. It took me more than an hour to pick the lock and honestly some luck to get them in the right position.

What made it work in the end was setting the first 4 pins and then using a rake to rake the last 2 into place. The reason why this worked is partially luck. After the first attempt putting the first pins into place was much harder. The truth is that only once there was a feel for the pins being set in place was I able to proceed but I believe they either did not stay in place on the sheer line or they were pushed above it when I was proceeding to further pins.


#### Conclusion

Lock picking is a proffesion in itself and by practicing and understanding more about how it works I have gained a better appreciation for the people that have mastered it. There are many parallels that can be drawn between lock picking and penetrating a machine inside a network. 

In the first place one must do reconissance about the lock that is being used, learn if there are any specific picks or techniques that need to be used.
Next you need to test out the lock. Try to get a feel for the pins and if possible analyse the key teeth to determine the relative position of all pins.
After understanding the lock you have multiple ways to get in based on the lock, bruteforcing (racking), specific exploits (per lock vulnerabilities), and common skill based techniques (pin picking).

Once a lock is unlocked the hack/ picking is successful. The one major difference being that you don't need to write a report for the lock, a demonstration will suffice.




