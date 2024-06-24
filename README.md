# Calculator
/*

main.c
Created on: Oct 12, 2023
 Author: hp
*/

#include "STD_TYPES.h" #include "BIT_MATH.h" #include "DIO_interface.h" #include <util/delay.h> #include "CALC_interface.h" #include "CLCD_interface.h" #include "KPD_interface.h"

u16 calc_add(u8 a[],u8 b[],u8 n,u8 m); u16 calc_sub(u8 a[],u8 b[],u8 n,u8 m); u16 calc_MUL(u8 a[],u8 b[],u8 n,u8 m); u16 calc_DIV(u8 a[],u8 b[],u8 n,u8 m); u8 Check (u8 Password[],u8 CheckPassword[],u8 NumberOfDigit); u8 KeyPad_Value=0; u8 NumberOFDigits=0; u8 FirstNum[4] ; u8 SecondNum[4] ; u8 Password[16] ; //16 because CLCD max 216 u8 CheckPassword[16]; u8 Counter=0; u8 Checker; u8 Operation; void main (void) { //CLCD DIO_voidSetPortDir(PORTA_REG,PORT_DIR_OUT); DIO_voidSetPortDir(PORTC_REG,PORT_DIR_OUT); //KPD DIO_voidSetPortDir(PORTB_REG,0b00001111); DIO_voidSetPortVal(PORTB_REG,PORT_VAL_HIGH); CLCD_voidInit(); CLCD_voidSendString("SET PASSWORD"); // SET PASSWORD while (1) { do { KeyPad_Value=KPD_u8GetPressedKey(); }while(KeyPad_Value =='\0'); //NULL NumberOFDigits++; if (KeyPad_Value =='&') break; CLCD_voidSendPos(1,NumberOFDigits-1); CLCD_voidSendNum(KeyPad_Value); _delay_ms(200); CLCD_voidSendPos(1,NumberOFDigits-1); CLCD_voidSendData(''); Password[NumberOFDigits]=KeyPad_Value; }

/*********************************************************/

	//CHECK PASSWORD
	while (1)
{
		CLCD_voidSendCommand(Clear);
		_delay_ms(100);
		CLCD_voidSendString("CHECK PASSWORD");
		 Counter =0;
		u8 KeyPad_Value='\0';
		while(KeyPad_Value !='&')
		{
	         do
		  {
	        KeyPad_Value=KPD_u8GetPressedKey();
		  }while(KeyPad_Value =='\0');     //NULL
	            Counter++;
		  if (KeyPad_Value =='&')
			 break;
		   CLCD_voidSendPos(1,Counter-1);
		   CLCD_voidSendNum(KeyPad_Value);
		   _delay_ms(200);
		   CLCD_voidSendPos(1,Counter-1);
	       CLCD_voidSendData('*');
	    CheckPassword[Counter]=KeyPad_Value;
		}
/*********************************************/ Checker= Check(CheckPassword,Password,Counter); if(Checker==NumberOFDigits-1) { CLCD_voidSendCommand(Clear); CLCD_voidSendPos(0,1); CLCD_voidSendString("LOADING"); for(u8 i=0;i<5;i++) // FOR LOOP BELONGS TO DOTS { CLCD_voidSendPos(1,i); CLCD_voidSendData('.'); _delay_ms(500); } _delay_ms(200); CLCD_voidSendCommand(Clear); CLCD_voidSendPos(0,1); CLCD_voidSendString("CALCULATOR"); CLCD_voidSendPos(1,0); CLCD_voidSendString("IS READY"); _delay_ms(2000); CLCD_voidSendCommand(Clear);

         while(1)
      {
    u8 Counter=0;
    u8 Operation=0;
    while(1)
    {
     do{
    	 KeyPad_Value=KPD_u8GetPressedKey();
    	 _delay_ms(200);
     }while(KeyPad_Value == '\0');
     if (KeyPad_Value =='+' || KeyPad_Value =='-' || KeyPad_Value=='*' || KeyPad_Value=='/')
     {
    	 Operation=KeyPad_Value;
    	 CLCD_voidSendPos(0,Counter+1);
    	 CLCD_voidSendData(KeyPad_Value);
    	 break;
     }

     FirstNum[Counter]=KeyPad_Value;
     CLCD_voidSendPos(0,Counter);
     CLCD_voidSendNum(KeyPad_Value);
      }
u8 Counter2=Counter+1;
u8 Counter3=0;
while(1)
{
	do
	{
		KeyPad_Value=KPD_u8GetPressedKey();
		_delay_ms(200);
	}while(KeyPad_Value =='\0');
	if (KeyPad_Value =='=')
	{
		CLCD_voidSendPos(0,Counter2+1);
		CLCD_voidSendData(KeyPad_Value);
		break;
	}
	SecondNum[Counter3]=KeyPad_Value;
	Counter2++;
	Counter3++;
	CLCD_voidSendPos(0,Counter2);
	CLCD_voidSendNum(KeyPad_Value);
}
switch(Operation) { case '+': CLCD_voidSendPos(0,Counter2+2); CLCD_voidSendNum(calc_add(FirstNum,SecondNum,Counter,Counter3)); break; case '-': CLCD_voidSendPos(0,Counter2+2); CLCD_voidSendNum(calc_sub(FirstNum,SecondNum,Counter,Counter3)); break; case '*': CLCD_voidSendPos(0,Counter2+2); CLCD_voidSendNum(calc_MUL(FirstNum,SecondNum,Counter,Counter3)); break; case '/': CLCD_voidSendPos(0,Counter2+2); CLCD_voidSendNum(calc_DIV(FirstNum,SecondNum,Counter,Counter3)); break; } _delay_ms(3000); CLCD_voidSendCommand(Clear); }

		}
	else
	  {
        CLCD_voidSendCommand(Clear);
        CLCD_voidSendString(" WRONG PASSWORD");
       _delay_ms(2000);
	   }

}
}

u8 Check (u8 Password[],u8 CheckPassword[],u8 NumberOfDigit) { u8 Checker=0; for(u8 i=0; i<NumberOFDigits-1;i++) { if(Password[NumberOfDigit]==CheckPassword[Counter]) { Checker++; } } return Checker; } u16 calc_add(u8 a[],u8 b[],u8 n,u8 m) { u16 Num1=a[0]; u16 Num2=b[0]; for(u8 Counter=1;Counter<n;Counter++) { Num1=Num110+a[Counter]; } for(u8 Counter=1;Counter<m;Counter++) { Num2=Num210+b[Counter]; } return (Num1+Num2); } u16 calc_sub(u8 a[],u8 b[],u8 n,u8 m) { u16 Num1=a[0]; u16 Num2=b[0]; for(u8 i=1;i<n;i++) { Num1=Num110+a[i]; } for(u8 j=1;j<m;j++) { Num2=Num210+b[j]; } return (Num1-Num2); }

u16 calc_MUL(u8 a[],u8 b[],u8 n,u8 m) { u16 Num1=a[0]; u16 Num2=b[0]; for(u8 Counter=1;Counter<n;Counter++) { Num1=Num110+a[Counter]; } for(u8 Counter=1;Counter<m;Counter++) { Num2=Num210+b[Counter]; } return (Num1Num2); } u16 calc_DIV(u8 a[],u8 b[],u8 n,u8 m) { u16 Num1=a[0]; u16 Num2=b[0]; for(u8 Counter=1;Counter<n;Counter++) { Num1=Num110+a[Counter]; } for(u8 Counter=1;Counter<m;Counter++) { Num2=Num2*10+b[Counter]; } return (Num1/Num2); }
