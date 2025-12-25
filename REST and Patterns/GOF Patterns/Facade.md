Provide entry/access point to client through façade, to a fairly complex system.

Façade is used when there are different systems doing different thing in your subsystem but most of your clients needs only a very basic representation of those things. So you expose that complex subsystem directly for advanced users, but also expose a façade, which provides a sensible defaults.

# Problems addressed

- To make a complex subsystem easier to use, a simple interface should be provided for a set of interfaces in the subsystem.
    
- The dependencies on a subsystem should be minimized.
    
- implements a simple interface in terms of (by delegating to) the interfaces in the subsystem and
    
- may perform additional functionality before/after forwarding a request.
![[Pasted image 20251224234114.png]]
```
public interface Hotel
{
    public Menus getMenus();
}


public class NonVegRestaurant implements Hotel
{
    public Menus getMenus()
    {
        NonVegMenu nv = new NonVegMenu();
        return nv;
    }
}

public class VegRestaurant implements Hotel
{
    public Menus getMenus()
    {
        VegMenu v = new VegMenu();
        return v;
    }
}

public class VegNonBothRestaurant implements Hotel
{
    public Menus getMenus()
    {
        Both b = new Both();
        return b;
    }
}

public class HotelKeeperFacade
{
    public VegMenu getVegMenu()
    {
        VegRestaurant v = new VegRestaurant();
        VegMenu vegMenu = (VegMenu)v.getMenus();
        return vegMenu;
    }
      
    public NonVegMenu getNonVegMenu()
    {
        NonVegRestaurant v = new NonVegRestaurant();
        NonVegMenu NonvegMenu = (NonVegMenu)v.getMenus();
        return NonvegMenu;
    }
      
    public Both getVegNonMenu()
    {
        VegNonBothRestaurant v = new VegNonBothRestaurant();
        Both bothMenu = (Both)v.getMenus();
        return bothMenu;
    }    
}

public class Client
{
    public static void main (String[] args)
    {
        HotelKeeper keeper = new HotelKeeper();
          
        VegMenu v = keeper.getVegMenu();
        NonVegMenu nv = keeper.getNonVegMenu();
        Both = keeper.getVegNonMenu();
  
    }
}
```
