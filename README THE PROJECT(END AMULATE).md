import random
import time


class LostAmuletGame:
    def __init__(self):
        self.score = 20  # Starting score to allow room for failure
        self.has_weapon = None  # None means no weapon yet
        self.has_key = False

    def print_pause(self, message, delay=2):
        """Prints a message with a delay for dramatic effect."""
        print(message)
        time.sleep(delay)

    def get_choice(self, prompt, choices):
        """Gets the player's choice from the given options."""
        while True:
            choice = input(prompt).strip().lower()
            if choice in choices:
                return choice
            print(f"Invalid choice. Please enter one of: {', '.join(choices)}")

    def randomize_items(self):
        """Randomizes the weapon the player encounters."""
        items = ["dagger", "sword", "bow"]
        return random.choice(items)

    def show_score(self):
        """Displays the current score to the player."""
        print(f"\nCurrent Score: {self.score}\n")

    def check_game_over(self):
        """Checks if the game should end based on score."""
        if self.score >= 30:
            self.game_over("You've mastered the temple!", victory=True)
            return True
        elif self.score <= 0:
            self.game_over("The darkness consumes you...", victory=False)
            return True
        return False

    def start(self):
        """Starts the game with the intro and path choices."""
        self.print_pause("\nThe End Amulet")
        self.print_pause(
            "\nYou are one of the Heroes who got separated while "
            "fighting a monster."
        )
        self.print_pause("\nThen you fell into a terrifying Temple.")
        self.print_pause(
            "\nThe temple puffs like a sleeping animal. Three paths "
            "tremble in the faint torchlight:"
        )
        self.print_pause("1) Left Path - Whispers curl like smoke through stone.")
        self.print_pause("2) Middle Path - A glowing weapon waits in shadows.")
        self.print_pause("3) Right Path - Stairs descend into dark, flowing water.")
        self.main_chamber()

    def main_chamber(self):
        """Handles the decision for which path the player takes."""
        if self.check_game_over():
            return
            
        self.show_score()
        choice = self.get_choice(
            "Which path do you follow? (1/2/3): ", ["1", "2", "3"]
        )
        if choice == "1":
            self.left_path()
        elif choice == "2":
            self.middle_path()
        elif choice == "3":
            self.right_path()

    def left_path(self):
        """Handles the left path where the player faces a misty whisper."""
        self.print_pause(
            "\nThe corridor narrows. Voices knot in your ears. "
            "A hand of cold mist encircles yours."
        )
        choice = self.get_choice(
            "\n1) Burn it with your torch\n2) Shut your eyes\n"
            "3) Lean in and listen\nSelect (1/2/3): ",
            ["1", "2", "3"],
        )

        if choice == "1":
            self.print_pause(
                "\nThe hand screams, then disappears. The flame roars briefly."
            )
            self.score += 5
        elif choice == "2":
            self.print_pause(
                "\nDarkness whisks you away. When you open your eyes, "
                "you are elsewhere - the Middle Path."
            )
            self.score += 2
            self.middle_path()
        elif choice == "3":
            self.print_pause(
                "\nA whisper in your ear: 'The dagger deceives. "
                "Truth waits beneath the tide.'"
            )
            self.score += 10
        self.main_chamber()

    def middle_path(self):
        """Handles the middle path where player encounters a weapon."""
        if self.check_game_over():
            return
            
        if not self.has_weapon:
            self.print_pause(
                "\nA figure stands holding a weapon. Its blade glows "
                "like an open sore."
            )
            self.has_weapon = self.randomize_items()
            self.print_pause(f"\nYou see a {self.has_weapon} waiting in shadows.")

        choice = self.get_choice(
            "\n1) Accept the weapon\n2) Refuse and remain firm\n"
            "3) Demand its name\nSelect (1/2/3): ",
            ["1", "2", "3"],
        )

        if choice == "1":
            self.print_pause(
                f"\nYou grasp the {self.has_weapon}. Energy vibrates "
                "through you like thunder."
            )
            self.score += 5
        elif choice == "2":
            self.print_pause("\nYou raise your arms. The figure strikes.")
            if self.has_key:
                self.print_pause("The key shines like the sun. You survive.")
                self.score += 8
            else:
                self.print_pause("Its blow is final. The silence is total.")
                self.score -= 10
        elif choice == "3":
            self.print_pause(
                "\nYou speak. The air becomes quiet. In that pause, "
                "you grab the weapon and escape."
            )
            self.score += 10
        self.main_chamber()

    def right_path(self):
        """Handles the right path with a serpent in dark water."""
        if self.check_game_over():
            return
            
        self.print_pause(
            "\nThe water below stirs. A serpent moves beneath, "
            "curling like cities in motion."
        )
        choice = self.get_choice(
            "\n1) Resist it\n2) Dive below\nSelect (1/2): ", ["1", "2"]
        )

        if choice == "1":
            if self.has_weapon:
                self.print_pause(
                    f"\nThe {self.has_weapon} warbles. You strike true. "
                    "The death of the beast resounds."
                )
                self.print_pause("Behind it is the Amulet of Shadows, glowing weakly.")
                self.score += 15
                self.game_over(
                    "You took the Amulet. Power is your name now.", victory=True
                )
            else:
                self.print_pause(
                    "\nYou charge with empty hands. The serpent dispatches you."
                )
                self.print_pause("Your body is broken, but not your spirit.")
                self.score += 2
                self.main_chamber()
        elif choice == "2":
            self.print_pause(
                "\nYou dive. The cold nips. Something sparkles under the silt: "
                "a golden key."
            )
            if not self.has_key:
                self.has_key = True
                self.score += 5
            self.print_pause("You emerge, panting. The key is warm in your hand.")
            self.main_chamber()

    def game_over(self, message, victory=False):
        """Handles the game over sequence with score and message."""
        self.print_pause(f"\n=== GAME OVER ===\n{message}")
        self.print_pause(f"Final Score: {self.score}")
        
        if victory:
            if self.score >= 30:
                self.print_pause("The temple remembers your name. You walk as legend.")
            elif self.score >= 15:
                self.print_pause("Shadows part at your passing. You are not forgotten.")
        else:
            self.print_pause("The temple forgets. Dust settles over your footsteps.")

        again = self.get_choice("\nWould you like to try again? (y/n): ", ["y", "n"])
        if again == "y":
            self.__init__()
            self.start()
        else:
            self.print_pause("The torch goes out. The story sleeps.")
            
if __name__ == "__main__":
    LostAmuletGame().start()
