    BOOL CH347InitI2C()
    {
    	BOOL RetVal = FALSE;
    	BOOL isStrentch = FALSE;
    	ULONG iMode;	// I2C模式
    	ULONG I2CDelayMs = 0;
    	
    	iMode = SendDlgItemMessage(SpiI2cGpioDebugHwnd,IDC_I2CCfg_Clock,CB_GETCURSEL,0,0);
    	isStrentch = SendDlgItemMessage(SpiI2cGpioDebugHwnd,IDC_I2CCfg_SclStretch,CB_GETCURSEL,0,0) ? FALSE : TRUE ;
    	I2CDelayMs = GetDlgItemInt(SpiI2cGpioDebugHwnd,IDC_I2CCfg_Delay,NULL,FALSE);
    	
    	RetVal = CH347I2C_Set(SpiI2cGpioDevIndex, iMode);
    	DbgPrint("CH347I2C Set clock %s",RetVal ? "succ" : "failure");
    
    	// 原始工程中未将 isStrentch 传入 CH347I2C_SetStretch 函数，导致无法正确设置时钟拉伸功能。现已设置为强制使能。
    	RetVal = CH347I2C_SetStretch(SpiI2cGpioDevIndex, TRUE);
    	DbgPrint("CH347 I2C set stetching %s",RetVal ? "succ" : "failure");
    
    	// goodhead额外增加推挽输出模式的配置，默认为推挽输出模式。
    	RetVal = CH347I2C_SetDriverMode(SpiI2cGpioDevIndex, 1);
    	DbgPrint("CH347 I2C set push-pull %s", RetVal ? "succ" : "failure");
    
    	if (I2CDelayMs > 0)
    		RetVal = CH347I2C_SetDelaymS(SpiI2cGpioDevIndex, I2CDelayMs);
    	
    	DbgPrint("CH347InitI2C %s",RetVal ? "succ" : "failure");
    
    	return RetVal;
    }
