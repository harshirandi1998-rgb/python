import random
import os

def load_question_bank(file_path):
    """
    Loads a text file and extracts Question/Answer pairs.
    """
    questions = []
    answers = []

    try:
        # FIX: Added encoding="utf-8"
        with open(file_path, "r", encoding="utf-8") as file:
            lines = [line.strip() for line in file.readlines() if line.strip()]

            for i in range(0, len(lines), 2):
                if i + 1 < len(lines):
                    questions.append(lines[i])
                    answers.append(lines[i + 1])

        return questions, answers, True

    except FileNotFoundError:
        return [], [], False

    except UnicodeDecodeError:
        print("Encoding error: Your file is not UTF-8. Please save it again using UTF-8 encoding.")
        return [], [], False


def main():
    print("Welcome to Professor Assistant version v1.0")
    name = input("Please Enter Your Name: ")
    print(f"Hello Professor {name}, I am here to help you create exams for question bank.\n")

    while True:
        choice = input("Do you want me to help you create an exam? (Yes to proceed | No to quit the program): ")

        if choice.lower() == "no":
            print(f"Thank you Professor {name}. Have a good day!")
            break

        elif choice.lower() == "yes":
            file_path = input("Please Enter the Path of the Question Bank: ")

            questions, answers, success = load_question_bank(file_path)

            if not success or len(questions) == 0:
                print("Error: Unable to read file or the file contains no data. Please try again.\n")
                continue

            print("Yes, indeed the path you provided includes questions and answers.")

            try:
                count = int(input("How many question-answer pairs do you want to include in your exam? "))
            except ValueError:
                print("Invalid entry. Please enter a valid number.\n")
                continue

            if count > len(questions):
                print(f"Only {len(questions)} pairs available. Selecting all of them.")
                count = len(questions)

            output = input("Where do you want to save your exam? ")

            chosen_indices = random.sample(range(len(questions)), count)

            try:
                # FIX: Added encoding="utf-8"
                with open(output, "w", encoding="utf-8") as exam_file:
                    for idx in chosen_indices:
                        exam_file.write(f"Q: {questions[idx]}\n")
                        exam_file.write(f"A: {answers[idx]}\n\n")

                print(f"Congratulations Professor {name}. Your exam is created and saved in: {output}\n")

            except Exception as e:
                print(f"Error saving exam file: {e}\n")

        else:
            print("Invalid option. Please type Yes or No.\n")


if __name__ == "__main__":
    main()
