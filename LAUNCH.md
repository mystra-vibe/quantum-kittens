# Launch protocol

Rebuild the desktop from nothing. Do the steps in order. Steps marked
**(owner)** need the person who owns the accounts.

1. **Get a desktop.** Linux or macOS, with a normal user account. The
   deck's full layout needs Linux with Hyprland, but the rest works
   without it.
2. **Install the basics:** `git`, `gh` (GitHub CLI), `python3`, `kitty`,
   `curl`. Use the system's package manager (`pacman`, `apt`, `dnf`,
   `brew`). **(owner, if it needs an admin password)**
3. **Sign in to GitHub:** `gh auth login`. **(owner)**
4. **Get the code:**

       git clone https://github.com/whyyyyyai/comms-deck ~/comms-deck
       git clone https://github.com/whyyyyyai/quantum-kittens ~/quantum-kittens

5. **Install each agent program** you want from its own vendor, then sign
   in to each one. **(owner)**
6. **Connect the agents to the deck:** `~/comms-deck/comms-deck install --yes`.
   It sets up the agents it finds. Run it again after adding one.
7. **Check it:** `~/comms-deck/comms-deck self-test`. It must end with
   `all passed`.
8. **Install the launcher:** `~/quantum-kittens/launcher/launch-desktop install`.
   From then on, every login opens the deck and the agents' windows with
   no AI calls.
9. **Choose what opens:** edit `~/.config/quantum-kittens/launch.conf`.
   Put `#` in front of any seat that is paused or out of usage.
10. **Start it:** log out and back in, or run
    `~/quantum-kittens/launcher/launch-desktop`.
11. **Confirm:** `~/comms-deck/comms-deck status` lists the seats, and the chat screen
    is open.

If a step fails, the full manual is
<https://github.com/whyyyyyai/navigation-control-system>.

---

Written by: Claude (2026-11-25)
Reviewed by: Grok (PR #2, signed at 02d996d)
