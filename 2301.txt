public class Calisma1 {
    
public STATIC INTEGER cntListOlustur(integer cntSayisi , string cntIsmi){
  
  list<contact> cntList = new List<contact>();
    for(integer i=0 ; i< cntSayisi ; i++      ){
       contact yeniRecord = new contact();
    
            yeniRecord.LastName  = cntIsmi + ' ' + i ;
            yeniRecord.AccountId = '001Ho000019qdZqIAI';
          cntList.add(yeniRecord);  
        
    }
    
    insert cntList;
   return   cntList.size();   
        
    }
    
//Calisma1.cntListOlustur(152,'AAA Wise');    
    
    
    
    // Bu hata veriyor, sanırım Close Date ile alakalı
    public static Opportunity createNewOpportunity(string name, decimal amount, string stage, date closeDate){
    Opportunity newRecord= new Opportunity();
    newRecord.Name= name;
    newRecord.Amount= amount;
    newRecord.StageName= stage;
   newRecord.CloseDate= closeDate;
    database.insert(newRecord);
    return newRecord;
    }
    
    
 //ornek 2
 // ISTEDIGINIZ SAYIDA CONTACT RECORDUNU OTOMAIK OLARAK OLUSTURAN BIR KOD YAZINIZ
// CONTACT ISIMLERI  CNT 1 , CNT 2 SEKLINDE OLSUN
// CONTACT ACCOUNTU OLARAK DA ISTEDIGINIZ BIR ACCOUNTU SECINIZ

    public static list<contact> createContacts(integer cntSayisi, string accName){
       
        list<contact> cntList = new List<contact>();
        string filtre = '%'+accName+'%';
        account acc = new account();
        
        try{
            
             acc =[SELECT id,name From Account 
                             WHERE name LIKE : filtre];
        }catch (Exception e){
            system.debug('Malesef '+accName+' isimli bir account yoktur.');
        }
        
        for(integer i=0 ; i<cntSayisi ; i++){
          contact cnt = new Contact();
            cnt.FirstName = 'AAA gene point';
            cnt.LastName = 'cnt ' + i ;
            if (acc.id != null){               
                cnt.AccountId = acc.id;  
            }
           
            cntList.add(cnt);
        }
        //INSERT cntList;
        return cntList;
       // BITTI
    }
    
// bütün contactlardan emaili olmayanlara email olarak 
// wisequarter@wisequarter ekleyelim.    
    
 public static list<contact> contactEmail(){
       
        list<contact> cntList = [SELECT id,name,Email  From contact];
       //string wiseEmail ='wise@wise.com';
    
        for(contact cnt : cntList ){
            if (cnt.Email  == null){
                
                cnt.Email  = 'wise@wise.com';
                
                
                    }else{
                        system.debug(cnt+' already has an email adress.');
                    }        
          
        }
        database.update(cntList,false) ;
        return cntList;
       
    }    
    
//annual revenue 500.000 in üzerinde olan account larin 
//rating ini hot olarak update edin.    
    
    public static list<account> accRating(){
       
        list<account> accList = [SELECT id, rating,AnnualRevenue   
                                 From account];
    	string rating= 'Hot';
    
        for(account acc : accList ){
            if (acc.AnnualRevenue  >= 500000){
                
                acc.rating  = rating;        
          
        }
           
   }
        database.update(accList,false) ;
        return accList;
 }       
    
    
//eger opp closed olmamis ve colse date 5 gun var ise 
//description a 'Ha Gayret' yazin.     
    
    public static list <Opportunity> oppCheck(){
       
        list<Opportunity> oppList = [SELECT id, CloseDate, Description, StageName   
                                     From Opportunity];
     
    		date bugun = date.today();
        	integer kalanGun ;
        	string DescriptionNote= 'HA GAYRET';
        
        for(Opportunity opp : oppList ){
             kalanGun = bugun.daysBetween(opp.CloseDate);
            //system.debug(kalanGun);
            if ((kalanGun <= 5 && kalanGun >= 0) &&  
                (opp.StageName !='Closed Won' && opp.StageName !='Closed Lost')){
                
                opp.Description=DescriptionNote ;
                   //system.debug(opp.Description);       
        	}    
    	}    
         database.update(oppList,false) ;
        return oppList;
    
    }
    
    
    
    
    
}