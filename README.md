class User:
    def __init__(self, name, age, weight, height, goal):
        self.name = name
        self.age = age
        self.weight = weight
        self.height = height
        self.goal = goal
        self.workout_history = []

    def update_weight(self, new_weight):
        self.weight = new_weight

    def update_goal(self, new_goal):
        self.goal = new_goal

    def log_workout(self, workout):
        self.workout_history.append(workout)

    def display_info(self):
        return f"Name: {self.name}, Age: {self.age}, Weight: {self.weight}kg, Height: {self.height}cm, Goal: {self.goal}"

class Exercise:
    def __init__(self, name, description, difficulty):
        self.name = name
        self.description = description
        self.difficulty = difficulty

    def display_exercise(self):
        return f"{self.name} ({self.difficulty}): {self.description}"

class Workout:
    def __init__(self, name):
        self.name = name
        self.exercises = []

    def add_exercise(self, exercise, sets, reps, rest_time):
        self.exercises.append({'exercise': exercise, 'sets': sets, 'reps': reps, 'rest_time': rest_time})

    def remove_exercise(self, exercise_name):
        self.exercises = [ex for ex in self.exercises if ex['exercise'].name != exercise_name]

    def view_workout(self):
        print(f"Workout: {self.name}")
        for ex in self.exercises:
            print(f"{ex['exercise'].name}: {ex['sets']} sets, {ex['reps']} reps, {ex['rest_time']} seconds rest")

class ProgressTracker:
    def __init__(self, user):
        self.user = user

    def log_workout(self, workout):
        self.user.log_workout(workout)

    def generate_report(self):
        total_workouts = len(self.user.workout_history)
        return f"{self.user.name} has completed {total_workouts} workouts."

def menu_system():
    user = None
    exercises = []

    while True:
        print("--- Fitness App Menu ---")
        print("1. Set Up Profile")
        print("2. Edit Profile")
        print("3. View Profile")
        print("4. Add Exercise to Library")
        print("5. View Exercises")
        print("6. Create/Modify Workout")
        print("7. Track Progress")
        print("0. Exit")

        choice = input("Select an option: ")

        if choice == "1":
            name = input("Enter your name: ")
            age = int(input("Enter your age: "))
            weight = float(input("Enter your weight (kg): "))
            height = float(input("Enter your height (cm): "))
            goal = input("Enter your fitness goal: ")
            user = User(name, age, weight, height, goal)
            tracker = ProgressTracker(user)
            print("Profile set up successfully.")

        elif choice == "2" and user:
            print("1. Update Weight")
            print("2. Update Goal")
            edit_choice = input("Select an option: ")
            if edit_choice == "1":
                new_weight = float(input("Enter new weight (kg): "))
                user.update_weight(new_weight)
            elif edit_choice == "2":
                new_goal = input("Enter new fitness goal: ")
                user.update_goal(new_goal)
            print("Profile updated successfully.")

        elif choice == "3" and user:
            print(user.display_info())

        elif choice == "4":
            name = input("Enter exercise name: ")
            description = input("Enter exercise description: ")
            difficulty = input("Enter difficulty level (Easy, Moderate, Hard): ")
            exercises.append(Exercise(name, description, difficulty))
            print("Exercise added to library.")

        elif choice == "5":
            if exercises:
                for exercise in exercises:
                    print(exercise.display_exercise())
            else:
                print("No exercises in the library.")

        elif choice == "6":
            if user:
                workout_name = input("Enter workout name: ")
                workout = Workout(workout_name)
                while True:
                    print("\n1. Add Exercise to Workout")
                    print("2. Remove Exercise from Workout")
                    print("3. View Workout")
                    print("0. Finish Workout")
                    workout_choice = input("Select an option: ")

                    if workout_choice == "1":
                        for idx, exercise in enumerate(exercises):
                            print(f"{idx + 1}. {exercise.display_exercise()}")
                        ex_choice = int(input("Select an exercise by number: ")) - 1
                        sets = int(input("Enter number of sets: "))
                        reps = int(input("Enter number of reps: "))
                        rest_time = int(input("Enter rest time (seconds): "))
                        workout.add_exercise(exercises[ex_choice], sets, reps, rest_time)
                    elif workout_choice == "2":
                        ex_name = input("Enter exercise name to remove: ")
                        workout.remove_exercise(ex_name)
                    elif workout_choice == "3":
                        print(workout.view_workout())
                    elif workout_choice == "0":
                        break
                tracker.log_workout(workout)
                print("Workout saved and logged.")

        elif choice == "7" and user:
            print(tracker.generate_report())

        elif choice == "0":
            print("Exiting the app. Stay fit!")
            break

        else:
            print("Invalid option or profile not set up.")

menu_system()
