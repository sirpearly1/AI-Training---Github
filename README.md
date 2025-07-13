# AI-Training---Github
Learning to create AI with Github 
I will be guided on a project to create an automated SBA for basic schools in ghana report cards
# Enhanced SBA & Terminal Report Card System for Ghana Basic Schools

SUBJECTS = [
    "English", "Mathematics", "Science", "Social", "RME", "Computing",
    "Creative Arts", "Career Technology", "History", "French"
]

def get_remark(score):
    if score < 50:
        return "Emerging"
    elif score < 60:
        return "Approaching Proficiency"
    elif score < 80:
        return "Proficient"
    else:
        return "Highly Proficient"

class Student:
    def __init__(self, name, student_id):
        self.name = name
        self.student_id = student_id
        # subject: {'offered': bool, 'test1': x, 'group_work': x, 'test2': x, 'project': x, 'exam': x}
        self.subjects = {subj: {'offered': False} for subj in SUBJECTS}

    def offer_subject(self, subject):
        if subject in self.subjects:
            self.subjects[subject]['offered'] = True

    def enter_score(self, subject, test1, group_work, test2, project, exam):
        if subject in self.subjects and self.subjects[subject]['offered']:
            self.subjects[subject].update({
                'test1': test1,
                'group_work': group_work,
                'test2': test2,
                'project': project,
                'exam': exam
            })

    def compute_scores(self, subject):
        data = self.subjects[subject]
        if not data['offered']:
            return None
        ca_total = data.get('test1', 0) + data.get('group_work', 0) + data.get('test2', 0) + data.get('project', 0)
        ca_50 = ca_total * 0.5
        exam_50 = data.get('exam', 0) * 0.5
        final_score = ca_50 + exam_50
        remark = get_remark(final_score)
        return {
            'ca_total': ca_total,
            'ca_50': ca_50,
            'exam': data.get('exam', 0),
            'exam_50': exam_50,
            'final_score': final_score,
            'remark': remark
        }

    def generate_report(self):
        print(f"\nTerminal Report Card for {self.name} (ID: {self.student_id})")
        print("-" * 70)
        for subject in SUBJECTS:
            if self.subjects[subject]['offered']:
                scores = self.compute_scores(subject)
                print(f"Subject: {subject}")
                print(f"  Continuous Assessment (CA): {scores['ca_total']} / 100 | 50%: {scores['ca_50']:.2f}")
                print(f"  Exam Score: {scores['exam']} / 100 | 50%: {scores['exam_50']:.2f}")
                print(f"  Final Subject Score: {scores['final_score']:.2f} / 100")
                print(f"  Remark: {scores['remark']}")
                print("-" * 70)
        print("")

def main():
    students = []
    print("Welcome to the Enhanced SBA & Terminal Report Card System")
    while True:
        print("\n1. Add Student")
        print("2. Mark Subjects Offered")
        print("3. Enter Scores")
        print("4. Generate Report Card")
        print("5. Exit")
        choice = input("Select an option: ")
        if choice == '1':
            name = input("Enter student name: ")
            student_id = input("Enter student ID: ")
            students.append(Student(name, student_id))
            print(f"Student {name} added.")
        elif choice == '2':
            student_id = input("Enter student ID: ")
            student = next((s for s in students if s.student_id == student_id), None)
            if student:
                print("Select subjects offered (type subject name exactly, comma-separated):")
                print("Available subjects:", ", ".join(SUBJECTS))
                offered = input("Enter subjects: ").split(",")
                for subj in offered:
                    subj = subj.strip()
                    if subj in SUBJECTS:
                        student.offer_subject(subj)
                        print(f"{subj} marked as offered.")
                    else:
                        print(f"{subj} not recognized.")
            else:
                print("Student not found.")
        elif choice == '3':
            student_id = input("Enter student ID: ")
            student = next((s for s in students if s.student_id == student_id), None)
            if student:
                for subject in SUBJECTS:
                    if student.subjects[subject]['offered']:
                        print(f"\nEntering scores for {subject}:")
                        test1 = float(input("  Test 1 (max 30): "))
                        group_work = float(input("  Group Work (max 20): "))
                        test2 = float(input("  Test 2 (max 30): "))
                        project = float(input("  Project (max 20): "))
                        exam = float(input("  Exam (max 100): "))
                        student.enter_score(subject, test1, group_work, test2, project, exam)
                        print(f"Scores entered for {subject}.")
            else:
                print("Student not found.")
        elif choice == '4':
            student_id = input("Enter student ID: ")
            student = next((s for s in students if s.student_id == student_id), None)
            if student:
                student.generate_report()
            else:
                print("Student not found.")
        elif choice == '5':
            print("Exiting. Goodbye!")
            break
        else:
            print("Invalid choice. Try again.")

if __name__ == "__main__":
    main()
