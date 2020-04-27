## Go-vim bindings

* `dif` - delete function body
* Imports on save
* **Split and join**  
	
  	When you press `gS` it will convert this structure `str := Foo{ Name: "Almas", Enabled: true }` to this one 
	
		str := Foo{
			Name: "Almas",
  			Enabled: true,
			}
   When you press `gJ` in will do conver it back
   
* **Commands**
	* **:GoDecls** - show all functions and structures 
	* **:GoInfo** - show which params method accepts
	* **:GoReferrers** - show all references to given var
	* **:RoRename** - rename 
* **Hot Keys**
	* `[[` and `]]` - go to next function
