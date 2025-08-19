BARKOD_TANIMLARI TABLOSUNDA Trigers klasöründe
Varyan_Barkod oluşturulacak
içerisine bu kod yazılacak


USE [MikroDesktop_BETUSHE]
GO
/****** Object:  Trigger [dbo].[Varyant_Barkod]    Script Date: 19.08.2025 17:37:54 ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
ALTER TRIGGER [dbo].[Varyant_Barkod]
   ON  [dbo].[BARKOD_TANIMLARI]
   AFTER INSERT, UPDATE
AS
BEGIN
    SET NOCOUNT ON;
     
DECLARE @V1 NVARCHAR(40), @V2 NVARCHAR(40),@v3 NVARCHAR(40),  @guid1  uniqueidentifier,@guid2  uniqueidentifier
   
   SELECT @V1=bar_MikroSpecial1, @V2=bar_MikroSpecial2,@v3=bar_MikroSpecial3  FROM inserted i -- gelen veri / incoming data
   
   select @guid1=VBag_Guid   from VARYANT_BAGLANTI_TANIMLARI WHERE  VBag_KirilimKod=@V1 and   VBag_Tip='0'
   select @guid2=VBag_Guid   from VARYANT_BAGLANTI_TANIMLARI WHERE  VBag_KirilimKod=@V2 and   VBag_Tip='1'

    
    BEGIN
	
        UPDATE dbo.BARKOD_TANIMLARI
            SET bar_VarBaglantiUId1=@guid1,bar_VarBaglantiUId2=@guid2-- yeni değer / new value
        FROM inserted WHERE inserted.bar_Guid=dbo.BARKOD_TANIMLARI.bar_Guid
       
    END
END
