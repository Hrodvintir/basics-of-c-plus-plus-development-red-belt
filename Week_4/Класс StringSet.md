# Класс StringSet
Пожалуйста, ознакомьтесь с классом StringSet, на разборе которого основано следующее видео.

Этот класс позволяет:

 - добавить строку с заданным приоритетом с помощью метода Add;

 - вернуть последнюю добавленную строку с помощью метода FindLast;

 - вернуть строку с максимальным приоритетом с помощью метода FindBest.
 ```c++
 #include "test_runner.h"
#include <iostream>
#include <vector>
#include <set>

using namespace std;

class StringSet {
public:
  void Add(const string& s, int priority) {
    data.push_back(s);
    sorted_data.insert(StringItem{s, priority});
  }
  const string& FindLast() const {
    return data.back();
  }
  const string& FindBest() const {
    return prev(sorted_data.end())->s;
  }

private:
  vector<string> data;

  struct StringItem {
    string s;
    int priority;
    bool operator<(const StringItem& other) const {
      return priority < other.priority;
    }
  };
  set<StringItem> sorted_data;
};

// Юнит-тесты

void TestSimple() {
  StringSet strings;
  strings.Add("upper", 10);
  strings.Add("lower", 0);
  ASSERT_EQUAL(strings.FindLast(), "lower");
  ASSERT_EQUAL(strings.FindBest(), "upper");
}

void TestAnother() {
  StringSet strings;

  strings.Add("apple", 1);
  strings.Add("orange", 3);
  ASSERT_EQUAL(strings.FindLast(), "orange");
  ASSERT_EQUAL(strings.FindBest(), "orange");

  strings.Add("plum", -5);
  ASSERT_EQUAL(strings.FindLast(), "plum");
  ASSERT_EQUAL(strings.FindBest(), "orange");

  strings.Add("lemon", 5);
  ASSERT_EQUAL(strings.FindLast(), "lemon");
  ASSERT_EQUAL(strings.FindBest(), "lemon");
}

int main() {
  TestRunner tr;
  RUN_TEST(tr, TestSimple);
  RUN_TEST(tr, TestAnother);
  return 0;
}
 ```
 Ту же задачу можно было решить проще, не храня все строки, но мы неявно предполагаем, что в дальнейшем функциональность этого класса расширится, и возникнет необходимость итерироваться по всем строкам: как в порядке добавления, так и в порядке приоритета.

Проще говоря, будем считать, что существует необходимость в хранении всех строк — от этого допущения пример не теряет своей актуальности.

