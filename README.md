#include <stdio.h>
#define NUM_COURSES 7
#define NUM_CLASSES_PER_SEMESTER 4
#define NUM_SEMESTERS 2

typedef struct {
    float td;
    float tp;
    float quiz;
    float exam;
} Course;

typedef struct {
    Course classes[NUM_CLASSES_PER_SEMESTER];
} Semester;

float computeFinalMark(Course *course) {
    float final_mark = 0.0;
    int num_elements = 0;

    if (course->td != -1) {
        final_mark += course->td;
        num_elements++;
    }
    if (course->tp != -1) {
        final_mark += course->tp;
        num_elements++;
    }
    if (course->quiz != -1) {
        final_mark += course->quiz;
        num_elements++;
    }
    final_mark += course->exam;
    num_elements++;

    return final_mark / num_elements;
}

float computeSemesterAverage(Semester *semester) {
    float total_mark = 0.0;
    for (int i = 0; i < NUM_CLASSES_PER_SEMESTER; ++i) {
        total_mark += computeFinalMark(&semester->classes[i]);
    }
    return total_mark / NUM_CLASSES_PER_SEMESTER;
}

void checkRemediation(Semester *semester) {
    printf("Courses for remediation:\n");
    for (int i = 0; i < NUM_CLASSES_PER_SEMESTER; ++i) {
        if (computeFinalMark(&semester->classes[i]) < 10.0) {
            printf("Course %d\n", i + 1);
        }
    }
}

void displaySemesterResult(Semester *semester) {
    float semester_average = computeSemesterAverage(semester);
    printf("Semester Average: %.2f\n", semester_average);
    if (semester_average < 10.0) {
        printf("Failed the semester.\n");
    } else {
        printf("Passed the semester.\n");
    }
}

void checkAcademicYearResult(Semester semesters[NUM_SEMESTERS]) {
    float total_average = 0.0;
    for (int i = 0; i < NUM_SEMESTERS; ++i) {
        total_average += computeSemesterAverage(&semesters[i]);
    }
    total_average /= NUM_SEMESTERS;
    printf("Academic Year Average: %.2f\n", total_average);
    if (total_average >= 10.0) {
        printf("Passed the academic year.\n");
    } else {
        printf("Failed the academic year.\n");
    }
}

int main() {
    Semester modules[NUM_SEMESTERS];

    for (int s = 0; s < NUM_SEMESTERS; ++s) {
        printf("Enter marks for Semester %d:\n", s + 1);
        for (int c = 0; c < NUM_CLASSES_PER_SEMESTER; ++c) {
            printf("Course %d (TD TP Quiz Exam): ", c + 1);
            scanf("%f %f %f %f", &modules[s].classes[c].td, &modules[s].classes[c].tp, &modules[s].classes[c].quiz, &modules[s].classes[c].exam);
        }
    }

    for (int s = 0; s < NUM_SEMESTERS; ++s) {
        printf("Results for Semester %d:\n", s + 1);
        displaySemesterResult(&modules[s]);
        checkRemediation(&modules[s]);
    }

    checkAcademicYearResult(modules);

    return 0;
}
