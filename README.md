import random

# Инициализация структур данных
students = [
    {'name': 'Апполон', 'characteristics': {'возраст': 15}},
    {'name': 'Ярослав', 'characteristics': {'возраст': 14}},
    {'name': 'Александра', 'characteristics': {'возраст': 14}},
    {'name': 'Дарья', 'characteristics': {'возраст': 15}},
    {'name': 'Ангелина', 'characteristics': {'возраст': 14}}
]

classes = [
    {'name': 'Математика', 'info': {'учитель': 'Иванова'}},
    {'name': 'Русский Язык', 'info': {'учитель': 'Петрова'}},
    {'name': 'Информатика', 'info': {'учитель': 'Мошнина'}}
]

grades = {}


def generate_initial_data():
    for student in students:
        name = student['name']
        grades[name] = {}
        for cls in classes:
            class_name = cls['name']
            grades[name][class_name] = [random.randint(2, 5) for _ in range(3)]


generate_initial_data()


def add_grade(student_name, class_name, grade):
    if student_name not in grades:
        print("Ученик не найден")
        return
    if not any(cls['name'] == class_name for cls in classes):
        print("Предмет не существует")
        return
    if class_name not in grades[student_name]:
        grades[student_name][class_name] = []
    grades[student_name][class_name].append(grade)


def remove_grade(student_name, class_name, index):
    try:
        del grades[student_name][class_name][index]
    except (KeyError, IndexError):
        print("Ошибка при удалении оценки")


def edit_grade(student_name, class_name, index, new_grade):
    try:
        grades[student_name][class_name][index] = new_grade
    except (KeyError, IndexError):
        print("Ошибка при редактировании оценки")


def calculate_average(student_name):
    if student_name not in grades:
        return None
    total = 0
    count = 0
    for class_name in grades[student_name]:
        total += sum(grades[student_name][class_name])
        count += len(grades[student_name][class_name])
    return total / count if count != 0 else 0


def add_student(name, characteristics=None):
    if any(s['name'] == name for s in students):
        print("Ученик уже существует")
        return
    students.append({'name': name, 'characteristics': characteristics or {}})
    grades[name] = {cls['name']: [] for cls in classes}


def remove_student(name):
    global students
    students = [s for s in students if s['name'] != name]
    if name in grades:
        del grades[name]


def update_student_characteristics(name, new_characteristics):
    for student in students:
        if student['name'] == name:
            student['characteristics'].update(new_characteristics)
            return
    print("Ученик не найден")


def add_class(class_name, class_info=None):
    if any(cls['name'] == class_name for cls in classes):
        print("Предмет уже существует")
        return
    classes.append({'name': class_name, 'info': class_info or {}})
    for student in grades:
        grades[student][class_name] = []


def remove_class(class_name):
    global classes
    classes = [cls for cls in classes if cls['name'] != class_name]
    for student in grades:
        if class_name in grades[student]:
            del grades[student][class_name]


def update_class_info(class_name, new_info):
    for cls in classes:
        if cls['name'] == class_name:
            cls['info'].update(new_info)
            return
    print("Предмет не найден")


def print_student_averages():
    for student in students:
        name = student['name']
        avg = calculate_average(name)
        print(f"{name}: Средний балл - {avg:.2f}")


# Пример использования
print("Начальные данные:")
print_student_averages()

# Добавляем новую оценку
add_grade('Ангелина', 'Математика', 5)
print("\nПосле добавления оценки:")
print_student_averages()

# Добавляем новый предмет
add_class('Информатика', {'учитель': 'Сидоров'})
add_grade('Ярослав', 'Русский Язык', 4)
print("\nПосле добавления предмета и оценки:")
print_student_averages()

# Добавляем нового ученика
add_student('Иннокентий', {'возраст': 15})
add_grade('Иннокентий', 'Математика', 4)
print("\nПосле добавления нового ученика:")
print_student_averages()

# Редактируем характеристику ученика
update_student_characteristics('Иннокентий', {'телефон': '123-456'})
print("\nХарактеристики Иннокентия:")
print([s for s in students if s['name'] == 'Иннокентий'][0]['characteristics'])
