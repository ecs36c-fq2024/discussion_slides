---
marp: true
---

# Discussion 1

## Agenda

* Introduction and Logistics
  + About the course
  + Tools
* Basic C++ Review
---

# Logistics

## Lectures

TR 09:00–10:20, Young 198

## Discussions

M 10:00–10:50, Young 184
W 10:00–10:50, Walker 1320
F 09:00–09:50, TLC 1218
F 12:10–13:00, Wellman 212

You may go to any discussion section.
We will *not* take attendance.

---

# Office Hours

## Instructor

T 08:00–09:00, Watershed 2211

## TA

* Ethan He (me):
  +  M  2:30 PM - 4:30 PM
  +  Kemper 53
* Jicheng Wang
  + TBD
  + TBD
---

# Homework
* There will be 4 homework.
* You have at least *3 weeks* to work on each.
* You have *unlimited* trails to submit & grade you work on Gradescope.
* We provide interface, you implement the functions.
* Test cases are not provided, you are responsible to test your code.
---

# Tools we use for homework
* Platform: Ubuntu 22.04.
  + On Windows: [Windows Subsystem for Linux](https://learn.microsoft.com/en-us/windows/wsl/install).
  + ssh to [CSIF](https://csif.cs.ucdavis.edu/).
  + [Docker](https://www.docker.com/). A [devcontainer](https://code.visualstudio.com/docs/devcontainers/containers) will be provided with each homework release.
* C++ version: C++11.
* Compiler: `g++` and `gdb`.
  + You are welcome to use `clang++`, but you have to make sure your code works on Gradescope.
* Editor: any of your choice.
  + I suggest [VS Code](https://code.visualstudio.com/) with [Remote-SSH](https://code.visualstudio.com/docs/remote/ssh) and [devcontainer](https://code.visualstudio.com/docs/devcontainers/containers) plugins.

---

# C++ Review

## Pointers & References

```cpp
void increment(int x) {
  x = x + 1;
}
int main() {
  int x = 0;
  increment(x);
  increment(x);
  std::cout << x << std::endl;
  return 0;
}
```

What is the output?
* 0

---

Using pointers

```cpp
void increment(int *x) {
  *x = *x + 1;
}
int main() {
  int x = 0;
  increment(&x);
  increment(&x);
  std::cout << x << std::endl;
  return 0;
}
```

---

Using reference

```cpp
void increment(int &x) {
  x = x + 1;
}
int main() {
  int x = 0;
  increment(x);
  increment(x);
  std::cout << x << std::endl;
  return 0;
}
```

---

### What's wrong with the code?

```cpp
struct Student {
  std::string name;
};

Student *create_student(std::string name) {
  Student s{name};
  return &s;
}
```

* [dangling pointer](https://en.wikipedia.org/wiki/Dangling_pointer#:~:text=Dangling%20pointers%20and%20wild%20pointers)

---

### How to fix it?

```cpp
struct Student {
  std::string name;
};

Student *create_student(std::string name) {
  Student *s = new Student{name};
  return s;
}
```

---

| Feature                 | Stack          | Heap              |
| :---------------------- | :------------- | :---------------- |
| Allocate                | local variable | `malloc` / `new`  |
| Deallocate              | Automatic      | `free` / `delete` |
| Access time             | Fast           | Slower            |
| Resize allocated memory | Not supported  | `realloc`         |
| Structure               | Linear         | Fragment          |

---

## Memory Errors

What are other memory errors other than dangling pointer?