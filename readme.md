<br/><br/>
<div align="center">
	<img width=200px"  src="https://github.com/parasail-lang/parasail/blob/main/documentation/logo/parasail_logo.svg" alt="Awesome">
</div>
<br><br>

# The ParaSail Programming Language

This is the main source code repository for ParaSail. It contains the compiler, the interpreter, the standard library, and documentation.

**Note: this README is for _users_ rather than _contributors_.
If you wish to _contribute_ to the compiler, you should read the
[Getting Started][getting_started] section of the parasail-dev-guide instead.
You can ask for help at [Gitter][gitter].**

[gitter]: https://gitter.im/parasail-lang/community
[getting_started]: https://github.com/parasail-lang/parasail/blob/main/parasail-dev-guide.md

```ada
import Clock;

interface Dining_Philosophers<Num_Phils : Univ_Integer := 5; Context<>> is

    func Dinner_Party (Length_Of_Party : Clock::Interval_Type; Context);

    type Philosopher_Index is new Integer<1..Num_Phils>;
    type Left_Or_Right is Enum<[#is_left_fork, #is_right_fork]>;

    concurrent interface Fork<> is  
        func Pick_Up_Fork(queued var F : Fork; Which_Fork : Left_Or_Right);
        func Put_Down_Fork(locked var F : Fork);
        func Create(Index : Philosopher_Index) -> Fork;
    end interface Fork;

    type Fork_Array is Array<Fork, Indexed_By => Philosopher_Index>;
        
end interface Dining_Philosophers;

class Dining_Philosophers<Num_Phils : Univ_Integer := 5; Context<>> is

  exports

    func Dinner_Party(Length_Of_Party : Clock::Interval_Type; Context) is
        Delay(Context.Clock.Wall_Clock, Length_Of_Party);
        Display(Context.IO.Standard_Output, "Dinner Party is over\n");
        return; 
      ||
        var Forks : Fork_Array := [for I in 1..Num_Phils => Create(I)];
        
        for Phil in Philosopher_Index concurrent loop
            const Left_Fork := Phil;
            const Right_Fork := Phil mod Num_Phils + 1;
           
            while True loop
                Display(Context.IO.Standard_Output, "Philosopher " | Phil | " is thinking\n");
                Delay(Clock, Next(Context.Random));  // Think
              then
                Pick_Up_Fork(Forks[Left_Fork], #is_left_fork);
              ||
                Pick_Up_Fork(Forks[Right_Fork], #is_right_fork);
              then
                Display(Context.IO.Standard_Output, "Philosopher " | Phil | " is eating\n");
                Delay(Clock, Next(Context.Random));  // Eat
              then
                Put_Down_Fork(Forks[Left_Fork]);
              ||
                Put_Down_Fork(Forks[Right_Fork]);
            end loop;
        end loop;
    end func Dinner_Party;
    
    concurrent class Fork<> is
        var Is_Available : Boolean;
        
      exports

        func Create(Index : Philosopher_Index) -> Fork is
            return (Is_Available => True);
        end func Create;
        
        func Pick_Up_Fork (queued var F : Fork; Which_Fork : Left_Or_Right) is 
          queued until F.Is_Available then
            F.Is_Available := False;
        end func Pick_Up_Fork;       
            
        func Put_Down_Fork(locked var F : Fork) is
            F.Is_Available := True;
        end func Put_Down_Fork;
        
    end class Fork;

end class Dining_Philosophers;
```

## Quick Start

- Read ["ParaSail: Less is More with Multicore"].
- To perfect your understanding read ["ParaSail: A Pointer-Free Pervasively-Parallel Language for Irregular Computations"].
	- Associated slide presentation ["Presentaton on ParaSail: Pointer-Free Pervasively-Parallel Language"].
- To gain reference knowledge read ["ParaSail: Reference Manual"].
- To follow along the original multi-year design process of ParaSail, read the blog ["Designing ParaSail, a new programming language"].

["ParaSail: Less is More with Multicore"]: https://github.com/parasail-lang/parasail/blob/main/documentation/parasail_intro.pdf   
["ParaSail: A Pointer-Free Pervasively-Parallel Language for Irregular Computations"]: https://github.com/parasail-lang/parasail/blob/main/documentation/Irregular_Computations_in_ParaSail.pdf
["Presentaton on ParaSail: Pointer-Free Pervasively-Parallel Language"]: https://github.com/parasail-lang/parasail/blob/main/documentation/programming_journal_2018/ParaSail_Programming_2019_slides.pdf
["ParaSail: Reference Manual"]: https://github.com/parasail-lang/parasail/blob/main/documentation/ref_manual/parasail_ref_manual.pdf
["Designing ParaSail, a new programming language"]: https://parasail-programming-language.blogspot.com


## Installing from Source

Todo

### Building on a Unix-like system

Todo

### Building on Windows

Todo

#### MinGW

?

#### Specifying an ABI

?

### Configure and Make

Todo

## Building Documentation

Todo

## Notes

Todo

## Getting Help

The ParaSail community congregates in a few places:

* [Stack Overflow] - Direct questions about using the language.
* [Gitter] - General discussion and broader questions.

[Stack Overflow]: https://stackoverflow.com/questions/tagged/parasail
[Gitter]: https://gitter.im/parasail-lang/community


## Contributing

If you are interested in contributing to the ParaSail project, please take a look
at the [Getting Started][getting_started] section of the parasail-dev-guide.



## License

ParaSail is primarily distributed under the terms of the ParaSail Copyright, the ParaSail Copyright 2 and the ParaSail Copyright Lib.

See [ParaSail Copyright], [ParaSail Copyright 2], and
[ParaSail Copyright Lib] for details.

[ParaSail Copyright]: https://github.com/parasail-lang/parasail/blob/main/parasail_copyright.txt
[ParaSail Copyright 2]: https://github.com/parasail-lang/parasail/blob/main/parasail_copyright2.txt
[ParaSail Copyright Lib]: https://github.com/parasail-lang/parasail/blob/main/parasail_copyright_lib.txt

## Trademark

todo
