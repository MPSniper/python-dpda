Python Deterministic Pushdown Automaton
===========

Python Deterministic Push Down Automaton<br>

This module has the following features:<br>
    * Creation of a PDA object with a designated start state<br>
    * Creation of multiple State objects, of the types: START, READ, PUSH, POP, ACCEPT<br>
    * Implicit rejection states; if a tape does not terminate in an ACCEPT state, it implicitly terminates in a REJECT state.
    * Add transitions to each state, which determine the behavior of the automaton<br>
    * Start a PDA by feeding it a tape (string) and a stack (list).<br> 
    * Print verbose output as the PDA proceeds through the input material.
<br><br>

The code in this README will assemble and test the following pushdown automaton.  We use '!' for the END symbol on both the tape and the stack.

![alt tag](http://i.imgur.com/jheP0Zl.png )

Usage
===========

Creating a PDA and States:
    
    from PDA import *
    from State import *
     
    # Create each state.
    accept5 = State(state_type="Accept")
    pop4 = State(state_type="Pop")
    pop3 = State(state_type="Pop")
    push2 = State(state_type="Push")
    read1 = State(state_type="Read")

    # When creating the 'start state', you must give the first 'processing' state of the PDA as the next_state argument.
    start_state = State(state_type="Start", next_state=read1)


    # Adding transitions to each state we've created.
    # READ states are given a character to read from the tape, and the next state they will go to if that character is read.
    # PUSH states are given a character to push to the stack, and the next state to go to after that.
    # POP states are given a character and a next state, and if they encounter that character when popping from the stack, the PDA proceeds to the next state.
    # ACCEPT states are not given any transitions; they are the endpoints of the PDA.
    read1.add_transition(push2, character="a")
    read1.add_transition(pop3, character="b")
    read1.add_transition(pop4, character="!")
    push2.add_transition(read1, character="X")
    pop3.add_transition(read1, character="X")
    pop4.add_transition(accept5, character="!")

    # Now that our states are set up, we create the PDA with the start state as the sole argument.
    my_pda = PDA(start_state)

Running a tape through the PDA to check language membership:


    word = "a"*42 + "b"*42

    # '!' will be our END symbol, so we append it to the end of the tape.
    my_tape = word + "!"

    # Create a stack containing only the END symbol.
    my_stack = ['!']

    # Start the PDA by giving it a tape and stack as input.
    my_pda.start(tape=my_tape, stack=my_stack)

Output
==========
The PDA will print whether the tape was accepted or rejected.  If the PDA constructor is given the parameter verbose=True, then the states will report via STDOUT whenever they perform any action.  Verbose mode is suggested for anyone trying to solidify their understanding of pushdown automata.

When running "my_tape" from the previous example, the following output is returned:


    Tape accepted!

Now, let's try giving the PDA a word that isn't in this language:


    # 42 'a' followed by only 21 'b'... this is not in our language!
    some_word = "a"*42 + "b"*21
    some_tape = some_word + "!"
    some_stack = ['!']
    my_pda.start(tape=some_tape, stack=some_stack)

Output:


    Tape NOT accepted!


Verbose Output
==========
If we run the PDA with verbose=True, as in this example:


    my_tape = "aabb!"
    my_stack = ['!']
    my_pda.start(tape=my_tape, stack=my_stack, verbose=True)


We get a much more complete output, reporting the step-by-step behavior of our automaton!


    TAPE:  aabb!
    STACK:  ['!']
    START -> next_state
    TAPE:  aabb!
    STACK:  ['!']
    READ a
    TAPE:  abb!
    STACK:  ['!']
    PUSH X
    TAPE:  abb!
    STACK:  ['!', 'X']
    READ a
    TAPE:  bb!
    STACK:  ['!', 'X']
    PUSH X
    TAPE:  bb!
    STACK:  ['!', 'X', 'X']
    READ b
    TAPE:  b!
    STACK:  ['!', 'X', 'X']
    POP X
    TAPE:  b!
    STACK:  ['!', 'X']
    READ b
    TAPE:  !
    STACK:  ['!', 'X']
    POP X
    TAPE:  !
    STACK:  ['!']
    READ !
    TAPE:  
    STACK:  ['!']
    POP !
    TAPE:  
    STACK:  []
    Tape accepted!
    FINAL TAPE:  
    FINAL STACK:  []
