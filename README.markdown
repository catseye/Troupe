Troupe
======

Troupe is an esolang designed by Chris Pressey on June 25, 2012, in Winnipeg,
Canada.

The name of this esolang is **Troupe** in the UK and Canada and Australia,
but in the USA, where they drop the "u" in words where it follows an "o", its
name is **Trope**.

Part I. Hedgehogs
-----------------

We have a troupe of hedgehogs.  Each has a position, a colour (in the USA:
color), a possible hedgehog to its left, and a possible hedgehog to its
right.  In this troupe, there is only one hedgehog which has no hedgehog to
its left, and only one hedgehog which has no hedgehog to its right, and the
following is true: for each hedgehog X, if Y is the hedgehog to X's right
(resp. left), then X is the hedgehog to Y's left (resp. right).  (i.e. the
hedgehogs are in a line formation.)

For some hedgehog X, the hedgehog to X's left and the hedgehog to X's right
are the *neighbours* (in the USA: *neighbors*) of X.

We may say that a hedgehog is "one of the hedgehogs to the left" (resp.
"right") of some other hedgehog; the meaning of this should be evident -- it
is just the transitive closure of "to its left" (resp. right).

All hedgehogs share a fixed speed at which they move (when they move), and a
fixed radius.  Also, the troupe consists of a finite number of hedgehogs at
all times, and their colours come from a finite set of possible colours that
can be distinguished from one another.  (And that there is one distinguished
"default" colour, which we will arbitrarily call "white".)

In the troupe, at any given time, there is either exactly one *leader*
hedgehog, or exactly one *leader-elect* hedgehog; never both and never
neither.

The leader hedgehog, when it exists, has a velocity.  It moves through space
with this velocity, changing its position.  All other hedgehogs in the troupe
try to "follow" it, which means the following:

Each hedgehog moves to minimize the distance between it and one of its
neighbours (up to a certain point: if the distance between it and its
neighbour is less than three times the hedgehog radius, it does not bother
to move at all.)

The neighbour it moves toward is the hedgehog on its left (resp. right), if
the leader is one of the hedgehogs to the left (resp. right).

Part II. Faery Rings
--------------------

We also have a set of faery rings.  Each ring has a fixed position, radius,
inner colour, and outer colour; it optionally has a signpost, a unit vector
pointing in any direction; and it optionally has an orientation, which is
either clockwise or counterclockwise.

Faery rings may not intersect each other *except* if both rings have
precisely the same position and radius *and* if they have different outer
colours.  (They may have the same inner colour.)

When a leader hedgehog intersects a faery ring (i.e. when the distance
between the hedgehog is less than the sum of the hedgehog radius and the
particular ring's radius), and if the outer colour of the faery ring is the
same as the colour of the leader hedgehog, the following things happen, in
this order:

*   The faery ring, and all faery rings which intersect it, become temporarily
    inactive, in that intersection with a hedgehog does not set off this same
    sequence of events until it becomes active again.

*   The leader hedgehog's colour is changed to the inner colour of the faery
    ring.

*   If the faery ring has a counterclockwise (resp. clockwise) orientation,
    then the hedgehog to the leader hedgehog's left (resp. right) becomes
    the leader-elect hedgehog.

    (If there *is* no hedgehog to the left (resp. right) of the leader
    hedgehog, then one pops into existence, at the same position as the
    leader hedgehog, and with a white colouring; and it becomes the
    leader-elect hedgehog.)

    When the set of hedgehogs has a leader-elect, the leader-elect moves to
    minimize its distance to the faery ring that made it leader-elect, and
    all other hedgehogs follow the leader-elect, as described above.

    Once the leader-elect hedgehog intersects the faery ring which made it
    the leader-elect (which it will eventually, as it moves to minimize the
    distance), it becomes the leader hedgehog.

*   If the faery ring has a signpost, the leader hedgehog is given a velocity
    in accordance with the direction of the signpost.

*   Once the leader hedgehog no longer intersects the faery ring, the faery
    ring, and all rings that it intersects, become active once again.

Part III. Hills
---------------

There are also any number of hills, each with a fixed position and radius.
These serve as goals; when the leader hedgehog reaches a hill, the troupe's
quest is over, and they are free to break formation and nibble on clover or
whatever it is that hedgehogs do for fun.

Part IV. Computational Class
----------------------------

The system outlined above should be Turing-complete, as it maps quite
readily to the definition of a Turing machine.

The line of hedgehogs serves as the tape, with their colours being symbols on
the tape, and the leader being the tape head.

The faery rings implement state transition rules, or rather, the parts of
transition rules that say "If the symbol on the tape is..." (which in our
system translates to "If the leader hedgehog is coloured...")  Overlapped
faery rings of all possible colours make for a traditional transition rule.

The state itself is encoded by the leader's position and velocity and which
faery ring they're headed toward next.  As long as the faery rings are
arranged appropriately, we can guarantee that, when the leader hedgehog
leaves a faery ring, possibly with a new velocity, it is bound to intersect
another faery ring eventually.  An example of an appropriate arrangement is:

*   The signpost in each faery ring points in a cardinal direction.

*   All faery rings have the same radius.

*   For each faery ring, there is another faery ring which shares at least
    one of its coordinates.

Finally, the hills serve as halt states.

Part V. Extras
--------------

We can certainly allow for the presence of elements in this world which
either do not affect the property of being Turing-complete, or if they do,
are not required to be present in any particular instance of the world.

### Rest Areas ###

A rest area has a fixed position and radius.

If the leader hedgehog enters a rest area, it does not move until no
hedgehog in the troupe is impelled to move (i.e. until all hedgehogs are
within three times the hedgehog-radius of another hedgehog.)  This allows
the troupe to "catch up" with the leader.

Part VI. Discussion
-------------------

This system is, of course, crying out to be implemented with a visual
animation of the hedgehogs moving around, possibly in a Java applet, or
in an HTML5 canvas with Javascript.

We have not specified the dimensionality of the space in which these things
exist; certainly, two dimensions would be a reasonable restriction,
especially for an animated visual implementation.

The system probably continues to be Turing-complete in one dimension, but
I have not convinced myself of that.  One of the things that allows this
(if it is true) is that hedgehogs are happily allowed to pass right through
other hedgehogs as if they didn't exist.  If this was not true, then a one-
dimensional realization of this world is probably not Turing-complete.

In two dimensions, however, even if hedgehogs are not allowed to pass
through each other, the system is still Turing-complete (cf. Beturing),
although we might have to add one proviso.  Since, in order to really be
Turing-complete, the troupe of hedgehogs needs to be able to grow without
bound, it might become intractably large.  When the leader hedgehog is not
able to make progress because of some other hedgehog in its way, it should
act as if it entered a rest area.  Also, we may require some "jitter" be
introduced in follower hedgehog movements, lest they get stuck.

We may want to go further, and allow the space itself to expand whenever a
hedgehog is added to the troupe -- so long as all the relative directions
between things are preserved, this should not affect the outcome.

(The idea for this system came to me sort of all at once when I was thinking
about Beturing, specifically that Beturing doesn't address the tape at all
in its concerns about wire-crossing.  I mean, don't we have to communicate
to the tape somehow, and might not those lines of communicate cross other
lines of communication?  Then I thought, well, it can assume that the tape
is *inside* the state pointer somehow.  And the rest followed more-or-less
naturally.)

We have not specified, either, whether this system should be implemented with
a discrete simulation or a continuous simulation.  Actually, either should
work, I think; and a continuous simulation would be somewhat interesting, if
only for the fact that most language interpreters are discrete.  A continuous
simulation would use real numbers to measure both duration and distance, and
real analysis techniques to discover the evolution of the system.  Motion of
the hedgehogs would be genuinely continuous except for the points where their
velocities change due to one hedgehog approaching another too closely, etc.

Nor have we said much about the topology of the space all of these things are
in.  Naturally, the space could be infinite, or it could be mapped onto a
torus so that the hedgehogs "wrap around" at the edges.  (The fact that the
space is finite is not a problem w.r.t. Turing-completeness, so long as there
are still an infinite number of possible coordinates, i.e. the space of real
or rational numbers; integers, though, would be problematic in a space of
finite extent.)  Or the space could be mapped onto the surface of a sphere,
with appropriate modifications.

We have also not constrained the sets of faery rings and hills to be finite;
so we could have an infinite number of them in an infinite space -- but for
correspondence with a Turing machine, these should probably be restricted to
infinite sets.  (Otherwise, if you have, say, an uncountable set of faery
rings, specified non-constructively, you could encode all possible
computations just in them, and the behaviour of the hedgehogs becomes
essentially moot.)

The initial condition could either consist of the troupe having a leader
hedgehog with an initial velocity, or a leader-elect hedgehog with an
initially-selected faery ring to which it is attracted.  The former is
probably more "natural" for some definition of "natural".
