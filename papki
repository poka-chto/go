package main

import (
	"bufio"
	"fmt"
	"os"
	"strconv"
	"strings"
)

// Task описывает одну задачу.
type Task struct {
	Title string
	Done  bool
}

// Folder описывает папку с задачами.
type Folder struct {
	Name  string
	Tasks []Task
}

// AddTask добавляет задачу в папку.
func (f *Folder) AddTask(title string) {
	task := Task{
		Title: title,
		Done:  false,
	}

	f.Tasks = append(f.Tasks, task)
}

// DeleteTask удаляет задачу по номеру.
func (f *Folder) DeleteTask(index int) bool {
	if index < 0 || index >= len(f.Tasks) {
		return false
	}

	f.Tasks = append(f.Tasks[:index], f.Tasks[index+1:]...)
	return true
}

// ToggleStatus меняет статус задачи.
func (t *Task) ToggleStatus() {
	t.Done = !t.Done
}

// Rename меняет название папки.
func (f *Folder) Rename(name string) {
	f.Name = name
}

// readLine читает строку из консоли.
func readLine(reader *bufio.Reader) string {
	text, _ := reader.ReadString('\n')
	return strings.TrimSpace(text)
}

// readNumber считывает число.
func readNumber(reader *bufio.Reader) (int, bool) {
	text := readLine(reader)
	number, err := strconv.Atoi(text)

	if err != nil {
		return 0, false
	}

	return number, true
}

// Проверяет, существует ли папка с таким названием.
func folderExists(folders []Folder, name string, except int) bool {
	for i, folder := range folders {
		if i != except && strings.EqualFold(folder.Name, name) {
			return true
		}
	}

	return false
}

// Меню выбранной папки.
func openFolder(reader *bufio.Reader, folder *Folder) {
	for {
		fmt.Printf("\n=== Папка: %s ===\n", folder.Name)
		fmt.Println("1. Показать задачи")
		fmt.Println("2. Создать задачу")
		fmt.Println("3. Изменить статус задачи")
		fmt.Println("4. Удалить задачу")
		fmt.Println("5. Назад")
		fmt.Println("0. Выход")
		fmt.Print("> ")

		choice, ok := readNumber(reader)
		if !ok {
			fmt.Println("Ошибка: введите номер пункта.")
			continue
		}

		switch choice {
		case 1:
			if len(folder.Tasks) == 0 {
				fmt.Println("В этой папке пока нет задач.")
				continue
			}

			fmt.Printf("\nЗадачи папки «%s»:\n", folder.Name)

			for i, task := range folder.Tasks {
				status := "Не выполнена"

				if task.Done {
					status = "Выполнена"
				}

				fmt.Printf("%d. %s [%s]\n", i+1, task.Title, status)
			}

		case 2:
			fmt.Print("Введите название задачи: ")
			title := readLine(reader)

			if title == "" {
				fmt.Println("Ошибка: название задачи не может быть пустым.")
				continue
			}

			folder.AddTask(title)
			fmt.Println("Задача создана!")

		case 3:
			if len(folder.Tasks) == 0 {
				fmt.Println("Нет задач для изменения статуса.")
				continue
			}

			fmt.Println("Выберите задачу:")
			for i, task := range folder.Tasks {
				fmt.Printf("%d. %s\n", i+1, task.Title)
			}

			fmt.Print("> ")
			number, valid := readNumber(reader)

			if !valid || number < 1 || number > len(folder.Tasks) {
				fmt.Println("Ошибка: задачи с таким номером нет.")
				continue
			}

			folder.Tasks[number-1].ToggleStatus()
			fmt.Println("Статус задачи изменён.")

		case 4:
			if len(folder.Tasks) == 0 {
				fmt.Println("Нет задач для удаления.")
				continue
			}

			fmt.Println("Выберите задачу для удаления:")
			for i, task := range folder.Tasks {
				fmt.Printf("%d. %s\n", i+1, task.Title)
			}

			fmt.Print("> ")
			number, valid := readNumber(reader)

			if !valid || number < 1 || number > len(folder.Tasks) {
				fmt.Println("Ошибка: задачи с таким номером нет.")
				continue
			}

			folder.DeleteTask(number - 1)
			fmt.Println("Задача удалена.")

		case 5:
			return

		case 0:
			fmt.Println("Выход из программы.")
			os.Exit(0)

		default:
			fmt.Println("Ошибка: выберите пункт от 0 до 5.")
		}
	}
}

func main() {
	reader := bufio.NewReader(os.Stdin)

	folders := []Folder{
		{Name: "Входящие"},
		{Name: "Работа"},
		{Name: "Личное"},
	}

	for {
		fmt.Println("\n=== Папки ===")

		for i, folder := range folders {
			fmt.Printf("%d. %s (%d задач)\n",
				i+1, folder.Name, len(folder.Tasks))
		}

		fmt.Println("4. Создать папку")
		fmt.Println("5. Удалить папку")
		fmt.Println("6. Переименовать папку")
		fmt.Println("0. Выход")
		fmt.Print("> ")

		choice, ok := readNumber(reader)
		if !ok {
			fmt.Println("Ошибка: введите номер пункта.")
			continue
		}

		switch {
		case choice >= 1 && choice <= len(folders):
			openFolder(reader, &folders[choice-1])

		case choice == 4:
			fmt.Print("Введите название новой папки: ")
			name := readLine(reader)

			if name == "" {
				fmt.Println("Ошибка: название папки не может быть пустым.")
				continue
			}

			if folderExists(folders, name, -1) {
				fmt.Println("Ошибка: такая папка уже существует.")
				continue
			}

			folders = append(folders, Folder{Name: name})
			fmt.Println("Папка создана!")

		case choice == 5:
			if len(folders) == 0 {
				fmt.Println("Нет папок для удаления.")
				continue
			}

			fmt.Print("Введите номер папки для удаления: ")
			number, valid := readNumber(reader)

			if !valid || number < 1 || number > len(folders) {
				fmt.Println("Ошибка: папки с таким номером нет.")
				continue
			}

			folders = append(folders[:number-1], folders[number:]...)
			fmt.Println("Папка и её задачи удалены.")

		case choice == 6:
			if len(folders) == 0 {
				fmt.Println("Нет папок для переименования.")
				continue
			}

			fmt.Print("Введите номер папки для переименования: ")
			number, valid := readNumber(reader)

			if !valid || number < 1 || number > len(folders) {
				fmt.Println("Ошибка: папки с таким номером нет.")
				continue
			}

			fmt.Print("Введите новое название: ")
			name := readLine(reader)

			if name == "" {
				fmt.Println("Ошибка: название не может быть пустым.")
				continue
			}

			if folderExists(folders, name, number-1) {
				fmt.Println("Ошибка: такая папка уже существует.")
				continue
			}

			folders[number-1].Rename(name)
			fmt.Println("Папка переименована!")

		case choice == 0:
			fmt.Println("Выход из программы.")
			return

		default:
			fmt.Println("Ошибка: такого пункта меню нет.")
		}
	}
}
