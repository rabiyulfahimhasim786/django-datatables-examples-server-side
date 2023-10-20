# django-datatables-examples-server-side
django-datatables-examples-server-side

#Reference - [link](https://tutorial101.blogspot.com/2022/04/python-django-how-to-use-datatables.html)


Python Django How To Use DataTables with Insert Data Jquery Ajax

Bootstrap 5
https://getbootstrap.com/docs/5.0/getting-started/introduction/
https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/css/bootstrap.min.css

Jquery
https://jquery.com/download/
CDN : jsDelivr CDN
https://www.jsdelivr.com/package/npm/jquery
https://cdn.jsdelivr.net/npm/jquery@3.6.0/dist/jquery.min.js

https://datatables.net/
DataTables is a powerful jQuery plugin for creating table listings and adding interactions to them. It provides searching, sorting and pagination without any configuration. 


 testdev/urls.py
 
 //testdev/urls.py
 from django.contrib import admin
 from django.urls import path
 from myapp import views
  
 urlpatterns = [
     path('', views.index, name='index'),
     path('insert', views.insert, name='insert'),
     path('admin/', admin.site.urls),
 ]
 myapp/views.py
 
 //myapp/views.py
 from django.shortcuts import render, redirect
 from .models import Member
  
# Create your views here.
def index(request):
    all_members = Member.objects.all()
    return render(request, 'datatables/index.html', {'all_members': all_members})
 
def insert(request):
    member = Member(firstname=request.POST['firstname'], lastname=request.POST['lastname'], address=request.POST['address'])
    member.save()
    return redirect('/')
myapp/models.py

   //myapp/models.py
   from django.db import models
    
   # Create your models here.
   class Member(models.Model):
       firstname = models.CharField(max_length=50)
       lastname = models.CharField(max_length=50)
       address = models.CharField(max_length=50)
     
       def __str__(self):
           return self.firstname + " " + self.lastname
           
   myapp/templates/datatables/index.html

     //myapp/templates/datatables/index.html
     {% extends 'datatables/base.html' %}
     {% block body %}
     <p>
     <button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#addnewmember" style="width:300px;">
       Add New
     </button>
     </p>
     <!-- Modal -->
     <div class="modal fade" id="addnewmember" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
       <div class="modal-dialog">
         <div class="modal-content">
           <div class="modal-header">
             <h5 class="modal-title" id="exampleModalLabel">Add New</h5>
             <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
           </div>
           <div class="modal-body">
             <form method="POST">
                 {% csrf_token %}
                 <div class="mb-3">
                     <label>Firstname</label>
                        <input type="text" id="firstname" class="form-control"/>
                 </div>        
                 <div class="mb-3">
                     <label>Lastname</label>
                     <input type="text" id="lastname" class="form-control"/>
                 </div>
                 <div class="mb-3">
                     <label>Address</label>
                     <input type="text" id="address" class="form-control"/>
                 </div>
              
           </div>
           <div class="modal-footer">
             <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
             <button type="button" class="btn btn-primary" id="submit">Submit</button>
           </div>
           </form>
         </div>
       </div>
     </div>
      
     <hr style="border-top:1px solid #000; clear:both;"/>
     <table id = "table" class = "table table-bordered">
         <thead class="alert-warning">
             <tr>
                 <th>Firstname</th>
                 <th>Lastname</th>
                 <th>Address</th>
             </tr>
         </thead>
         <tbody>
             {% for member in all_members %}
             <tr>
                 <td>{{ member.firstname }}</td>
                 <td>{{ member.lastname }}</td>
                 <td>{{ member.address  }}</td>
             </tr>
             {% endfor %}
         </tbody>
     </table>
     {% endblock %}
myapp/templates/datatables/base.html
?
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
44
45
46
47
48
49
50
51
52
53
54
55
56
57
//myapp/templates/datatables/base.html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Python Django How To Use DataTables with Insert Data Jquery Ajax</title>
    <link rel="stylesheet" type="text/css" href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/css/bootstrap.min.css"/>
    <link rel = "stylesheet" type = "text/css" href = "https://cdn.datatables.net/1.11.5/css/jquery.dataTables.min.css"/>
    <meta charset="UTF-8" name="viewport" content="width=device-width, initial-scale=1"/>
</head>
<body>
<div class="container">
    <div class="row">
        <p><h3 class="text-primary">Python Django How To Use DataTables with Insert Data Jquery Ajax</h3></p>
        <hr style="border-top:1px dotted #ccc;"/>
        {% block body %}
  
        {% endblock %}
    </div>
</div>    
</body>
<script src="https://cdn.jsdelivr.net/npm/jquery@3.6.0/dist/jquery.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/js/bootstrap.bundle.min.js"></script>
<script src = "https://cdn.datatables.net/1.11.5/js/jquery.dataTables.min.js"></script>
<script type = "text/javascript">
$(document).ready(function(){
    $('#table').DataTable();
     
    $('#submit').on('click', function(){
        $firstname = $('#firstname').val();
        $lastname = $('#lastname').val();
        $address = $('#address').val();
  
        if($firstname == "" || $lastname == "" || $address == ""){
            alert("Please complete field");
        }else{
            $.ajax({
                type: "POST",
                url: "insert",
                data:{
                    firstname: $firstname,
                    lastname: $lastname,
                    address: $address,
                    csrfmiddlewaretoken: $('input[name=csrfmiddlewaretoken]').val()
                },
                success: function(){
                    alert('Save Data');
                    $('#firstname').val('');
                    $('#lastname').val('');
                    $('#address').val('');
                    window.location = "/";
                }
            });
        }
    });
});
</script>
</html>
Register App myapp
devtest/settings.py
?
1
2
3
4
5
6
7
8
9
10
//devtest/settings.py
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'myapp',
]

migration
C:\my_project_django\testdev>python manage.py makemigrations
C:\my_project_django\testdev>python manage.py migrate
