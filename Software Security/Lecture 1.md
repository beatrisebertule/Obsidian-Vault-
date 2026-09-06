```c++
char* copy_and_print(char* string) {
char* b = malloc(strlen(string));
strcpy(b,string); // copy string to b
printf(”The string is %s.”, b);
free(b);
return(b);
}
int sum_using_pointer_arithmetic(int a[]) {
int sum = 0;
int *pointer = a;
for (int i=0; i<4; i++ ){
sum = sum + *pointer;
pointer++; }
return sum;
}
```

understand code like this

software stands on a 
platform - mostly OS and web browsers

CVE list
common vulenrability enumeration
known vulnerabilities

KEV knwon exploitable vulnerabilities

homework - look at stuff