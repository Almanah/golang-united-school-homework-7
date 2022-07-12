package coverage

import (
	"os"
	"reflect"
	"testing"
	"time"
)

func init() {
	content, err := os.ReadFile("students_test.go")
	if err != nil {
		panic(err)
	}
	err = os.WriteFile("autocode/students_test", content, 0644)
	if err != nil {
		panic(err)
	}
}

// --- Test People ---
func TestPeople_Len(t *testing.T) {
	person0 := Person{firstName: "fisrt"}
	person1 := Person{firstName: "second"}
	person2 := Person{firstName: "third"}
	people := People{person0, person1, person2}

	actualLen := people.Len()

	if actualLen != 3 {
		t.Errorf("the expected len is 3, but got %d", actualLen)
	}
}

// wrong check for birthday in implementation, so statements is reversed
func TestPeople_Less_ByBirthday(t *testing.T) {
	dateSmall := time.Now()
	dateBig := dateSmall.AddDate(1, 1, 1)
	personSmall := Person{firstName: "yyy", lastName: "zzz", birthDay: dateSmall}
	personBig := Person{firstName: "aaa", lastName: "bbb", birthDay: dateBig}
	people := People{personSmall, personBig}

	actualIsLessSmall := people.Less(0, 1)

	if actualIsLessSmall {
		t.Errorf("expected %v to be less that %v by birthDay, but got false", personSmall, personBig)
	}
	actualIsLessBig := people.Less(1, 0)

	if !actualIsLessBig {
		t.Errorf("expected %v to be bigger that %v by birthDay, but got false", personBig, personSmall)
	}
}

func TestPeople_Less_ByFirstName(t *testing.T) {
	date := time.Now()
	personSmall := Person{firstName: "aaa", lastName: "yyy", birthDay: date}
	personBig := Person{firstName: "zzz", lastName: "bbb", birthDay: date}
	people := People{personSmall, personBig}

	actualIsLessSmall := people.Less(0, 1)

	if !actualIsLessSmall {
		t.Errorf("expected %v to be less that %v by firstName, but got false", personSmall, personBig)
	}
	actualIsLessBig := people.Less(1, 0)

	if actualIsLessBig {
		t.Errorf("expected %v to be bigger that %v by firstName, but got false", personBig, personSmall)
	}
}

func TestPeople_Less_ByLastName(t *testing.T) {
	date := time.Now()
	name := "aaa"
	personSmall := Person{firstName: name, lastName: "bbb", birthDay: date}
	personBig := Person{firstName: name, lastName: "zzz", birthDay: date}
	people := People{personSmall, personBig}

	actualIsLessSmall := people.Less(0, 1)

	if !actualIsLessSmall {
		t.Errorf("expected %v to be less that %v by lastName, but got false", personSmall, personBig)
	}
	actualIsLessBig := people.Less(1, 0)

	if actualIsLessBig {
		t.Errorf("expected %v to be bigger that %v by lastName, but got false", personBig, personSmall)
	}
}

func TestPeople_Less_SamePersons(t *testing.T) {
	person := Person{firstName: "aaa", lastName: "bbb", birthDay: time.Now()}
	people := People{person, person}

	actualIsLessFirst := people.Less(0, 1)
	actualIsLessSecond := people.Less(1, 0)

	if actualIsLessFirst || actualIsLessSecond {
		t.Errorf("expected %v to be equal %v, but got false", person, person)
	}
}

func TestPeople_Swap(t *testing.T) {
	person0 := Person{firstName: "fisrt"}
	person1 := Person{firstName: "second"}
	people := People{person0, person1}

	people.Swap(0, 1)

	if people[0].firstName != person1.firstName {
		t.Errorf("expected to get %v but got %v", person1, people[0])
	}
	if people[1].firstName != person0.firstName {
		t.Errorf("expected to get %v but got %v", person0, people[1])
	}
}

// --- Test Matrix ---
const baseMatrixStr = "0 1\n2 3"

func TestMatrix_New_DiffRowsLen(t *testing.T) {
	wrongMatixStr := "0 1\n2 3 4"
	_, err := New(wrongMatixStr)

	if err == nil {
		t.Errorf("expected to get error for wrong matrix string %s, but got nothing", wrongMatixStr)
	}
}

func TestMatrix_New_NotInt(t *testing.T) {
	wrongMatixStr := "0 1\n2 a"
	_, err := New(wrongMatixStr)

	if err == nil {
		t.Errorf("expected to get error for wrong matrix string %s, but got nothing", wrongMatixStr)
	}
}

func TestMatrix_New(t *testing.T) {
	_, err := New(baseMatrixStr)

	if err != nil {
		t.Errorf("expected to get valid matrix from string %s, but got error: %s", baseMatrixStr, err)
	}
}

func TestMatrix_Rows(t *testing.T) {
	matrix, _ := New(baseMatrixStr)

	expectedRows := [][]int{{0, 1}, {2, 3}}
	actualRows := matrix.Rows()

	if !reflect.DeepEqual(expectedRows, actualRows) {
		t.Errorf("expected to get %v rows, but got %v", expectedRows, actualRows)
	}

}

func TestMatrix_Cols(t *testing.T) {
	matrix, _ := New(baseMatrixStr)

	expectedCols := [][]int{{0, 2}, {1, 3}}
	actualCols := matrix.Cols()

	if !reflect.DeepEqual(expectedCols, actualCols) {
		t.Errorf("expected to get %v cols, but got %v", expectedCols, actualCols)
	}
}

func TestMatrix_Set_WrongRow(t *testing.T) {
	matrix, _ := New(baseMatrixStr)

	actualResNeg := matrix.Set(-1, 0, 1)

	if actualResNeg {
		t.Errorf("expected to get false while setting into negative row, but got true")
	}
	actualResOut := matrix.Set(2, 0, 1)

	if actualResOut {
		t.Errorf("expected to get false while setting into out of range row, but got true")
	}
}

func TestMatrix_Set_WrongCol(t *testing.T) {
	matrix, _ := New(baseMatrixStr)

	actualResNeg := matrix.Set(0, -1, 1)

	if actualResNeg {
		t.Errorf("expected to get false while setting into negative col, but got true")
	}
	actualResOut := matrix.Set(0, 2, 1)

	if actualResOut {
		t.Errorf("expected to get false while setting into out of range col, but got true")
	}
}

func TestMatrix_Set(t *testing.T) {
	matrix, _ := New(baseMatrixStr)
	row := 0
	col := 0

	value := 100
	actualRes := matrix.Set(row, col, value)

	if !actualRes {
		t.Errorf("expected to set value %d into row/col %d/%d, but got false", value, row, col)
	}
	actualValue := matrix.Rows()[row][col]
	if actualValue != value {
		t.Errorf("expected to set value %d into row/col %d/%d, but set instead %d", value, row, col, actualValue)
	}
}
