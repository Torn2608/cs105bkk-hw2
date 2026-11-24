# HW2 notes

Answer each question in a few sentences. Be specific: name the function, the error or the command.

## What did you try?

I filled in the missing parts of `errors.py`, `event.py` and `club.py`. In `errors.py` I wrote `AlreadySignedUpError` and `UnknownEventError` as subclasses of `SignupError`. Each one calls `super().__init__(...)` with a message and keeps `member`, `event` or `title` as attributes. In `event.py` I wrote `Event.sign_up` (seat if `seats_left() > 0`, otherwise the waitlist), `Event.cancel` (removes the member and promotes the first person on the waitlist with `pop(0)`) and `Event.place`. In `club.py` I wrote `add_event`, `event`, `handle` and `attendance`. `attendance` uses `Counter.update` from the standard library. I checked everything by running `python club.py`, which runs the demo at the bottom of the file.

## What broke, and how did you fix it?

The first problem was lookup in `Club.event`. A missing title raises `KeyError`, but the chat bot only knows about `SignupError`. I catch `KeyError` and raise `UnknownEventError(title) from None`. Now `handle` can catch every sign-up problem with a single `except SignupError`, and the user sees "Sorry, there is no event called 'Chess night'." instead of a traceback. I did the same in `Event.place`, where `list.index` raises `ValueError`, and turned it into `NotSignedUpError`. I only catch the one error I expect, not a bare `except`. The second problem was `cancel`. I had to check the waitlist before the seats. Otherwise a waitlisted member would hit the "not signed up" error. I also had to promote someone only when a seat was freed, not when a waitlisted member leaves. The demo output looks right now: Tashi gets Nino's seat.

## What would you do differently next time?

I would write small tests before the code, for example with `pytest`. For now I only have the demo run, so I haven't tested edge cases like a duplicate `join`, `leave` by someone who isn't signed up, or a badly formatted command line. I would also think about the event titles. `handle` splits on commas, so a title with a comma in it can't work. I'd pick a different separator or note that limit.
