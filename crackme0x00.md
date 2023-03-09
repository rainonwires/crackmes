

    IOLI Crackme Level 0x00
    Password: 123
    Invalid Password!

Looking for a password, assumed to be stored in memory.

    0040133A | C70424 00404000          | mov dword ptr ss:[esp],crackme0x00.404000       | 404000:"IOLI Crackme Level 0x00\n"

Above it moves the **dword 404000** with a **pointer** and makes it **esp**, printing below.

    00401341 | E8 EA190000              | call <JMP.&printf>                              |
    00401346 | C70424 19404000          | mov dword ptr ss:[esp],crackme0x00.404019       | 404019:"Password: "

same thing as last.

    0040134D | E8 DE190000              | call <JMP.&printf>                              |
    00401352 | 8D45 E8                  | lea eax,dword ptr ss:[ebp-18]                   |
    00401355 | 894424 04                | mov dword ptr ss:[esp+4],eax                    | [esp+4]:"PE"
    00401359 | C70424 24404000          | mov dword ptr ss:[esp],crackme0x00.404024       | 404024:"%s"
    00401360 | E8 BB190000              | call <JMP.&scanf>                               |

at addr `0040134D` it calls `printf` asking for input. 
at addr `00401352` it calculates the memory address **ebp-18** as **eax** in the form of a **pointer dword**.
at addr `00401355` it moves a **pointer dword**. *(Interesting because of esp+4)*
at addr `00401359` it moves our input as a **pointer dword**
at addr `00401360` it scans again.


    00401365 | 8D45 E8                  | lea eax,dword ptr ss:[ebp-18]                   |
    00401368 | C74424 04 27404000       | mov dword ptr ss:[esp+4],crackme0x00.404027     | [esp+4]:"PE", 404027:"250382"
    00401370 | 890424                   | mov dword ptr ss:[esp],eax                      |
    00401373 | E8 98190000              | call <JMP.&strcmp>                              |
    00401378 | 85C0                     | test eax,eax                                    |
    0040137A | 74 0E                    | je crackme0x00.40138A                           |

addresses `00401365 - 00401370` calculate and assign memory one of which is **esp+4, 404027** (the password)
at addr `00401373` it compares our input '**esp**' with the password **str esp+4** 
not entirely sure what **test** **eax, eax** does. perhaps its referring to **crackme0x00.404027** in address **00401368** and the **eax** at
the end of address **00401370**

lastly addr `0040137A` is a simple **jump if equal to** addr `40138A` *(i think.)* This is saying if the test was equal to each other
**then jump to** **404041** which reads **"Password OK :)\n"**

    0040137C | C70424 2E404000          | mov dword ptr ss:[esp],crackme0x00.40402E       | 40402E:"Invalid Password!\n"
    00401383 | E8 A8190000              | call <JMP.&printf>                              |
    00401388 | EB 0C                    | jmp crackme0x00.401396                          |
    0040138A | C70424 41404000          | mov dword ptr ss:[esp],crackme0x00.404041       | 404041:"Password OK :)\n"
    00401391 | E8 9A190000              | call <JMP.&printf>                              |
    00401396 | B8 00000000              | mov eax,0                                       |
    0040139B | C9                       | leave                                           |
    0040139C | C3                       | ret                                             |

this leaves us with the string from the address `00401368`. I'm not ENTIRELY sure if this is correct, however the string contained
in that address, **esp+4** is the password. 

    IOLI Crackme Level 0x00
    Password: 250382
    Password OK :)

Excuse any lingo errors im not versed in what everything is called yet and i am using ioli to learn crackmes.
