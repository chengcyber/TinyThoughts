# Compile OCAML

- `ocamlc`
- `ocamlopt`

```
ocamlc -o file file.ml
ocamlc unix.cma -o file file.ml
```

# Record Type

```
(* Create a type of "person" records *)
type person = {
  first_name: string;
  last_name: string;
  address: string;
  weight: float;
  birthday: int;
}

let print_person_info () =

  let someone = {
    first_name: "cheng",
    last_name: "liu",
    address: "Some Where",
    weight: 75.5,
    birthday: 1234,
  } in
  Printf.printf "First name is %s - weight is %f \n"
  someone.fisrt_name someone.weight

;;

print_person_info()
```

Mutable field in Record
```
type person = {
  mutable fisrt_name: string;
  mutable last_name: string;
}

let someone = {
  first_name = "cheng";
  last_name = "liu";
}

someone.first_name <- "CHENG";;
someone;;

```

# Union

```
type number =
| Zero
| Integer of int
| Real of float
;;

let float_of_number n =
  match n with
    | Zero -> 0.0
    | Integer i -> float_of_int i
    | Real x -> x
;;

let z = Zero
let i = Integer 6
let d = Real 3.14
;;
float_of_number z;;
(* 0. *)
float_of_number i;;
(* 6. *)
float_of_number d;;
(* 3.14 *)
```

# Option (Some & None)

- no NULL or nil in OCAML
- build-in type 'a option
- type 'a option = None | Some of 'a ;;
- None intend to represent NULL, while Some handles non-NULL

```
let int_div x y =
  match y with
    | 0 -> failwith "Can't divide by 0"
    | _ -> x / y
;;

let d = int_div 10 0;;
(* Exception: Faliure "Can't divide by 0" */
```

```
let int_div x y =
  match y with
    | 0 -> None
    | _ -> Some (x / y)
```

```
let content x =
  match x with
    | Some c -> c
    | None -> 0
;;
```

# Module

- less likely variable name conflict
- compiled file will be a Module automatically naming with file name in capital case
- Module name must be capital case

```
module Module1 = struct
  let name = "m1"
end

module Module2 = struct
  let name= "m2"
end

Printf.printf "Module 1 is %s, Module 2 is %s\n"
  Module1.name Module2.name
;;
```

# Read file content

```
(* Function to read a file line by line *)
let read_file file_name =
  let in_channel = open_in file_name in
  try
    while true do
      let line = input_line in_channel in
        print_endline line
    done
  with End_of_file ->
    close_in in_channel

let my_fun () =
  let f = "file.ml" in
    read_file f

;;

my_fun ()
```

# OCAML File Extensions

| Purpose         | Bytecode | Native code |
| Source code     | `*.ml`   |             |
| Header files    | `*.mli`  |             |
| Object files    | `*.cmo`  | `*.cmx`     |
| Library files   | `*.cma`  | `*cmxa`     |
| Binary programs | prog     | prog.opt    |

# OCAML Scriping

- `#use` loads a source file into the current scope
- `#load` loads a module from a compiled bytecode file or bytecode library which use an extension of `.cma`
- `open` allows any values defined within a module to be used without being prefixed by the module name

```
#load "unix.cma"

let t = Unix.localtime (Unix.time ()) in
  let (day, month, year) = (t.tm_mday, t.tm_mon, t.tm_year) in
    Printf.printf "The current date is %d-%d-%d"
    (1900 + year) (month + 1) day

;;
```
`ocamlc unix.cma -o file file.ml`

```
open Printf

(* Printf.printf *)
printf
```

```
#!/usr/bin/ocaml
#load "unix.cma"
open Printf

let t = Unix.localtime (Unix.time ()) in
  let (day, month, year) = (t.tm_mday, t.tm_mon, t.tm_year) in
    Printf.printf "The current date is %d-%d-%d"
    (1900 + year) (month + 1) day

;;
```
`./file.ml`
