let express=require('express');
let app=express();
let cors=require('cors');
let bodyparser=require('body-parser');
let sha1=require('sha1');
let PORT=6677;

// first Task for check username or password

app.post('/adminlogin',(req,res)=>
{
    
    let username=req.body.username;
    let pass=sha1(req.body.password);//basically sh1 use for save password in encrypted form
    adminModel.findOne({username:username,password:pass},(err,data)=>   //adminModel bascially database file mame present in mongoDb
    {
        if(err){
            res.json({'err':1,'msg':'Something Went Wrong'})
        }
        else if(data==null)
        {
            res.json({'err':1,'msg':'username or Password is not correct'})
        }
        else 
        {
            res.json({'err':0,'msg':'Login Success'});
        }
    })


// second task is create admin via with email or pass

app.post('/employees', function (req, res) {
  let ins=new adminModel({email:email,password:pass}); // we save create data in database
  ins.save(err=>
    {
        if(err){
             res.json({'err':1,'msg':'Already Registerd'})
        }
      else 
        {
            res.json({'err':0,'msg':' Registerd'})
        }
   })
});

// third task edit or update data
app.post("/editdataa",(req,res)=>
{
    let username=req.body.username;
    let uid=req.body.uid;
   adminModel.updateOne({_id:uid},{$set:{Name:username}},(err)=>
    {
        if(err){
             res.json({'err':1,'msg':"some error"})
       }
        else 
        {
            res.json({'err':0,'msg':"username and uid updated"})
        }
    })
})

// 4th task is delete data

app.get("/delete",(req,res)=>
{
   let uid=req.params.uid;
  adminModel.deleteOne({_id:uid},(err)=>
   {
       if(err){
           res.json({'err':0,'msg':"some error found"})

      }
       else 
       {
           res.json({'err':0,'msg':'user details  deleted'})
       }
   })
})

// 5th task is forget password 
app.get("/forgetpassword",(req,res)=>
{
    let email=req.params.email;
    adminModel.findOne({email:email},(err,data)=>
    {
        if(err){
}
        else if(data==null)
        {
            res.json({"err":1,"msg":"Email not found"})
        }
        else 
        {
            let link="http://localhost:4200/forgetpassword/"+email;
            let options={
                from: '"Fred Foo"', // sender address
                to: email, // list of receivers
               subject: "Forget Password Link✔", // Subject line
               html: `Hello user , <br/>
                 Please click on the link for forget password <a href="${link}">Click Here</a>` // html body
                 }
                transporter.sendMail(options,(err,info)=>
                {
                    if(err){
                res.json({'err':1,'msg':'Some error'})
                    }
                    else 
                    {
                        res.json({'err':0,'msg':'Check your mail for link'});  
                    }
                }) 
        }
    })
})



// port define 6677 at top of code
app.listen(PORT,()=>
{
    console.log(`Work on ${PORT}`);
})
})
