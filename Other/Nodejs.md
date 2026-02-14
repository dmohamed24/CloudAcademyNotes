
- NodeJS is a JS rune time which basically m,eans that a its a programme environment that can run other programmes. Js apps can be run on the conmputer or te server
- Its build on top of ther V8 js engine that chrome uses. Takes Js code for then the machine to uderstand
- Takes JS out of the browser
- Modules import/wexport  - common js - requires older version & es modles impoort newwer verion
- Node js is none blocking whihc means it doent wait for any IO operation (netwrok calls, files/data operations) so instead of blocking the execution of code while weaiting for these to execute to completye Nodejs uses evetns and callbacks an this allows it to hanle thousands of connections at the same time whihc is way its so fast an efficient.
- There only 1 sginle thread that executes js code (thread a set of instructions the compoter ecxecute). In a single thtreaded env tgheres only 1 set of instructions that are execeuted at a time whihc is different to multi threadded env with multiple threads that can execute different parts of the code in parralle simultaniosly 
- Nodejs uses the event loop whhcih is a mechanism that allwas node to perform non-blocking IO operatoips.
- For example when you make a netwrk request, node js doesnt wait for ythat req to complete instead it continues to execute the rest of the code and when that request does complete it triggers a callback whihc is the added to the event queue and the event loop picks up the callback andf then executes it.
- 