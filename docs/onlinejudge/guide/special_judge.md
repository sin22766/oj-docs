### What is Special Judge

Special Judge means that OJ will use a specific program to determine whether the output of the submitted program is correct, rather than simply seeing whether the output of the submitted program is exactly the same as the standard output.

### scenes to be used

Special Judge is generally used because the answer to the question is not unique. More specifically, there are generally two situations:

 -The problem ultimately requires output of a solution, and this solution may not be unique.

 -The question finally requires the output of a floating-point number, and will tell as long as the difference between the answer and the standard answer does not exceed a certain small number, such as 0.01. In this case, it is possible to keep 3 decimal places, 4 decimal places, etc., and there is no harm in keeping a few more decimal places.

### Special Judge Judgment Procedure Example

```c
#include <stdio.h>

#define AC 0
#define WA 1
#define ERROR -1

int spj(FILE *input, FILE *user_output);

void close_file(FILE *f){
    if(f != NULL){
        fclose(f);
    }
}

int main(int argc, char *args[]){
    FILE *input = NULL, *user_output = NULL;
    int result;
    if(argc != 3){
        printf("Usage: spj x.in x.out\n");
        return ERROR;
    }
    input = fopen(args[1], "r");
    user_output = fopen(args[2], "r");
    if(input == NULL || user_output == NULL){
        printf("Failed to open output file\n");
        close_file(input);
        close_file(user_output);
        return ERROR;
    }

    result = spj(input, user_output);
    printf("result: %d\n", result);
    
    close_file(input);
    close_file(user_output);
    return result;
}

int spj(FILE *input, FILE *user_output){
    /*
      parameter:
        -input, the file pointer of the standard program input
        -user_output, pointer to user output file
      return:
        -If the user's answer is correct, return to AC
        -If the user's answer is wrong, return to WA
        -If you actively capture your own errors, such as memory allocation failure, return ERROR
      Ask the user to complete this function.
      demo:
      int a, b;
      while(fscanf(f, "%d %d", &a, &b) != EOF){
          if(a -b != 3){
              return WA;
          }
      }
      return AC;
     */
}
```

### Instructions

When testing locally, compile first, and then `./spj your_in_file_path user_output_path`.

When running on OJ, the judgment result comes from the return value of the `main` function. It should be noted that when the OJ judgement program is also running in the sandbox, the time limit is 3 times the time limit of the question, and the memory limit is 1G.
