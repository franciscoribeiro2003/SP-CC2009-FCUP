Firstly we checked if the site was sensible to XSS attacks using a basic html line "<script> alert("XSS"); </script>" :

![Screenshot from 2023-06-02 18-23-58](https://github.com/DCC-FCUP-SP/sp2223-t02g10/assets/92749121/d3c8c6d8-0af1-42eb-974d-000e5f4503f5)

We found out that it was:

![Screenshot from 2023-06-02 18-24-14](https://github.com/DCC-FCUP-SP/sp2223-t02g10/assets/92749121/d18f7433-5cdf-4f94-8ad2-f772b3e56f97)

So then we have to inspect the page with "ctrl + shift + c" to check the id of the button:

![Screenshot from 2023-06-02 18-29-23](https://github.com/DCC-FCUP-SP/sp2223-t02g10/assets/92749121/780a22de-2d97-48ad-b2a2-8dfff2233445)

Now we just had to remotly press the desired butto with the following line "<script> document.getElementById("giveflag").click(); </script>":

![Screenshot from 2023-06-02 18-10-59](https://github.com/DCC-FCUP-SP/sp2223-t02g10/assets/92749121/91b1b815-5791-4bd1-a7ff-e94e4f237496)

Then we got the flag:

![Screenshot from 2023-06-02 18-28-37](https://github.com/DCC-FCUP-SP/sp2223-t02g10/assets/92749121/18fa18d3-5e1e-480a-9557-0e74a891c87f)
