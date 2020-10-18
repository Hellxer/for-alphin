var express = require("express");
var login = require("./routes/loginroutes");
var bodyParser = require("body-parser");
var nodemailer = require("nodemailer");
var session = require("express-session");
var alert = require("./routes/alert");
var chat = require("./routes/chat");



const path = require("path");
var app = express();
app.use(
  session({
    secret: "secret",
    resave: true,
    saveUninitialized: true,
  })
);
app.use(bodyParser.urlencoded({ extended: true }));
app.use(bodyParser.json());
// app.use(function (req, res, next) {
//   res.header("Access-Control-Allow-Origin", "*");
//   res.header(
//     "Access-Control-Allow-Headers",
//     "Origin, X-Requested-With, Content-Type, Accept"
//   );
//   next();
// });
var router = express.Router();
// // test route
// app.get("/", function (req, res) {
//   res.json({ message: "welcome to our upload module apis" });
// });
app.set("view engine", "ejs");
//route to handle user registration
app.post("/registeruser", login.registeruser);
app.post("/registeradmin", login.registeradmin);
app.post("/login", login.login);
app.post("/codecheck", login.codecheck);
app.post("/forgot", login.forgotpassword);
app.post("/sendalert",alert.sendalert)
app.get("/getalert",alert.getalert)
app.post("/updatecode", login.updatecode);
app.post("/showcode", login.showcode);
app.post("/uploadimage", login.uploadimage);
app.post("/getalbum", login.getalbum);
app.get("/getimages/:resid", login.getimages);
app.get("/getdirectory/:resid", login.getdirectory);
app.get("/getnotnum/:wholeid", login.getnotnum);
app.get("/addnot", login.add);
app.get("/changenot/:wholeid", login.changenot);
app.get("/deleteimages/:eventid", login.deleteimageurls);
app.post("/createmeet", login.createmeet);
app.get("/getmeet/:wholeid", login.getmeet);
app.get("/getmeetcount/:wholeid", login.getmeetcount);
app.get("/delmeet/:meetid", login.delmeet);
app.get("/phonenumber/:userid", login.phonenumber);
app.get("/changepassword/:email", login.changepassword);
app.get("/deletephonenumber/:dirid", login.deletephonenumber);
app.get("/showcodeforresidence/:resid", login.showcodeforresidence);
app.post("/addphonenumber", login.addphonenumber);
app.get("/gettest/:resid", login.gettest);
app.get("/deletetest/:id", login.deletetest);
app.get("/delalert/:alertid", alert.delalert);

app.get("/getalerts/:resid",alert.getalert);
app.get("/getalertcount/:resid",alert.getalertcount);
app.get("/getroomadmin/:resid",chat.getroomadmin);
app.get("/getchat/:roomid",chat.getchat);
app.get("/getchatusers/:userid",chat.getchatusers);


app.post("/sendchat",chat.sendchat);

app.get("/note",login.note);
var pathfromlogin = __dirname;
module.exports = pathfromlogin;
app.use("/api", router);
app.use(express.static("public/"));
app.listen(4000);

app.get("/residinn", function (req, res) {
  res.render("residinn");
});

app.get("/reset/:token", login.resettokenget);

app.post("/reset/:token", login.resettokenpost);
app.get("/password-changed", function (req, res) {
  res.sendFile(path.join(__dirname + "/treat.html"));
});
