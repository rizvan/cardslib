# Cards Library: Header

In this page you can find info about:

* [Creating a base CardHeader](#creating-a-base-cardheader)
* [Standard Header without Buttons](#standard-header-without-buttons)
* [Standard Header with the overflow botton and Popup Menu](#standard-header-with-the-overflow-botton-and-popup-menu)
* [Standard Header with the overflow botton and PopupMenu built programmatically](#standard-header-with-the-overflow-botton-and-popupmenu-built-programmatically)
* [Standard Header with the expand/collpase button](#standard-header-with-the-expandcollpase-button)
* [Standard Header with custom button](#standard-header-with-custom-button)
* [Customizing the innerContent Header Layout](#customizing-the-innercontent-header-layout)
* [Customizing the HeaderLayout](#customizing-the-headerlayout)
* [Style](#style)


### Creating a base CardHeader

Creating a base `CardHeader` is pretty simple.

``` java
  //Create a Card
  Card card = new Card(getContext());

  //Create a CardHeader
  CardHeader header = new CardHeader(getContext());

  //Add header to card
  card.addCardHeader(header);
```

`CardHeader` provides a base HeaderLayout with a InnerLayout and ButtonLayout.

For the **CardViewNative**:
You can find it in [`res/layout/native_base_header_layout.xml`](/library-core/src/main/res/layout/native_base_header_layout.xml)

For the **CardView**:
You can find it in [`res/layout/base_header_layout.xml`](/library-core/src/main/res/layout/base_header_layout.xml)


The built-in ButtonLayout provides these features:

* a **overflow button** with popup and listener
* a **expand/collapse** button
* a **customizable button** for your custom action

The built-in InnerLayout defined by :
- [`res/layout/native_inner_base_header.xml`](/library-core/src/main/res/layout/native_inner_base_header.xml) (for the `CardViewNative`)

- [`res/layout/inner_base_header.xml`](/library-core/src/main/res/layout/inner_base_header.xml) (for the `CardView`) 

It provide these features:

* a **title**

There are many ways you can customize the card header.

### Standard Header without Buttons

If you want a `CardHeader` without buttons you can use this simple code:

``` java
        //Create a CardHeader
        CardHeader header = new CardHeader(getContext());
        card.addCardHeader(header);
```

![Screen](/demo/images/header/native/nobotton.png)

### Standard Header with the overflow botton and Popup Menu

If you want a `CardHeader` with the overflow button you can use this simple code:

``` java
        //Create a CardHeader
        CardHeader header = new CardHeader(getContext());

        //Set the header title
        header.setTitle(getString(R.string.demo_header_basetitle));

        //Add a popup menu. This method sets OverFlow button to visibile
        header.setPopupMenu(R.menu.popupmain, new CardHeader.onClickCardHeaderPopupMenuListener(){
            @Override
            public void onMenuItemClick(BaseCard card, MenuItem item) {
                Toast.makeText(getActivity(), "Click on "+item.getTitle(), Toast.LENGTH_SHORT).show();
            }
        });

        //Add a PopupMenuPrepareListener to add dynamically a menu entry
        //It is optional.
        header.setPopupMenuPrepareListener(new CardHeader.OnPrepareCardHeaderPopupMenuListener() {
            @Override
            public boolean onPreparePopupMenu(BaseCard card, PopupMenu popupMenu) {
                popupMenu.getMenu().add("Dynamic Item");
                return true;

                /*
                 * Other examples:
                 * You can remove an item with this code:
                 * popupMenu.getMenu().removeItem(R.id.action_settings);
                 *
                 * You can use return false to hide the button and the popup
                 * return false;
                 */

            }
        });
        card.addCardHeader(header);
```

You can use the listener `CardHeader.OnClickCardHeaderPopupMenuListener` to listen callback when an item menu is clicked.

You can set a `CardHeader.OnPrepareCardHeaderPopupMenuListener` with `setPopupMenuPrepareListener` method or with `setPopupMenu(int menuRes, OnClickCardHeaderPopupMenuListener listener, OnPrepareCardHeaderPopupMenuListener prepareListener)` to customize the menu items  after being inflated.

As described [below](#style), the overflow icon is defined with a style and you can customize it.

![Screen](/demo/images/header/native/overflow.png)


### Standard Header with the overflow botton and PopupMenu built programmatically

You can add a PopupMenu entirely from code:

``` java
        //Add a popup menu. This method sets OverFlow button to visible
        //If you would like to add all menu entries without xml file you have to set this value
        header.setButtonOverflowVisible(true);
        //Add the listener
        header.setPopupMenuListener(new CardHeader.OnClickCardHeaderPopupMenuListener() {
            @Override
            public void onMenuItemClick(BaseCard card, MenuItem item) {
                Toast.makeText(getActivity(), "Click on " + item.getTitle() + "-" + ((Card) card).getCardHeader().getTitle(), Toast.LENGTH_SHORT).show();
            }
        });

        //Add a PopupMenuPrepareListener to add dynamically a menu entry
        header.setPopupMenuPrepareListener(new CardHeader.OnPrepareCardHeaderPopupMenuListener() {
            @Override
            public boolean onPreparePopupMenu(BaseCard card, PopupMenu popupMenu) {
                popupMenu.getMenu().add("Dynamic item");
                return true;
            }
        });
```

You can see an example in `'HeaderFragment':
[(source native)](/demo/stock/src/main/java/it/gmariotti/cardslib/demo/fragment/nativeview/NativeHeaderFragment.java).
[(source v1)](/demo/stock/src/main/java/it/gmariotti/cardslib/demo/fragment/v1/HeaderFragment.java).


### Standard Header with the expand/collpase button

If you want a `CardHeader` with the expand/collapse button you can use this simple code:

``` java
        //Create a CardHeader
        CardHeader header = new CardHeader(getContext());

        //Set the header title
        header.setTitle(getString(R.string.demo_header_basetitle));

        //Set visible the expand/collapse button
        header.setButtonExpandVisible(true);

        //Add Header to card
        card.addCardHeader(header);

        //This provides a simple (and useless) expand area
        CardExpand expand = new CardExpand(getContext());

        //Set inner title in Expand Area
        expand.setTitle(getString(R.string.demo_expand_basetitle));

        card.addCardExpand(expand);
```

![Screen](/demo/images/header/native/expand.png)

If you want to customize the expand area you can simply extend `CardExpand` class. See this [page](EXPAND.md) for more information.

``` java
        //This provides a simple (and useless) expand area
        CustomExpandCard expand = new CustomExpandCard(getContext());

        //Add Expand Area to a Card
        card.addCardExpand(expand);
```

![Screen](/demo/images/header/expandCustom.png)

### Standard Header with custom button

`CardHeader` provides an 'other button' features.

You can use it for any actions.

you can use this simple code:

``` java
        //Create a CardHeader
        CardHeader header = new CardHeader(getContext());

        //Set the header title
        header.setTitle(getString(R.string.demo_header_basetitle));

        //Set visible the expand/collapse button
        header.setOtherButtonVisible(true);

        //Add a callback
        header.setOtherButtonClickListener(new CardHeader.OnClickCardHeaderOtherButtonListener() {
            @Override
            public void onButtonItemClick(Card card, View view) {
                Toast.makeText(getActivity(), "Click on Other Button", Toast.LENGTH_LONG).show();
            }
        });

        //Add Header to card
        card.addCardHeader(header);
```

You have to set your style and drawable for this button.

The easiest way is to copy this style in your project and use your custom drawables.

res/values/styles.xml : (I suggest you using a selector)
``` xml
    
    <!-- Other Button in Header for CardViewNative-->
    <style name="card.native.header_button_base.other">
         <item name="android:background">@drawable/card_menu_button_other</item>
    </style>
    
    <!-- Other Button in Header for CardView-->
    <style name="card.header_button_base.other">
        <item name="android:background">@drawable/card_menu_button_other</item>
    </style>

```
res/values-v21/styles.xml : (I suggest a simple image because it use a ripple as background)
``` xml
    <!-- Other Button in Header for CardViewNative-->
    <style name="card.native.header_button_base.other">
        <item name="android:src">@drawable/ic_menu_other_action_more_normal</item>
    </style>
    
    <!-- Other Button in Header for CardView-->
    <style name="card.header_button_base.other">
        <item name="android:src">@drawable/ic_menu_other_action_more_normal</item>
    </style>

```    


You can use the listener  `CardHeader.OnClickCardHeaderOtherButtonListener` to listen callback when the other button is clicked.

![Screen](/demo/images/header/native/otherbutton.png)

If you want to set the other button programmatically you can use the same code above and add:

``` java
        //Use this code to set your drawable
        if (Build.VERSION.SDK_INT >= Constants.API_L) {
            // Use the simple png. It is the src in image (Android-L uses the ripple)
            header.setOtherButtonDrawable(R.drawable.ic_action_add);
        } else {
            // Use a selector. It is the background in image
            header.setOtherButtonDrawable(R.drawable.card_menu_button_other_add);
        }
```

![Screen](/demo/images/header/native/otherbuttonAdd.png)

### Customizing the innerContent Header Layout

You can customize the built-in innerContent. You can **use your inner layout**.

You can extend the base class.

Then you can inflate your inner layout, then you can populate your layout with `setupInnerViewElements(ViewGroup parent, View view)` method

``` java
    public class CustomHeaderInnerCard extends CardHeader {

        public CustomHeaderInnerCard(Context context) {
            super(context, R.layout.carddemo_example_inner_header);
        }

        @Override
        public void setupInnerViewElements(ViewGroup parent, View view) {

            if (view!=null){
                TextView t1 = (TextView) view.findViewById(R.id.text_example1);
                if (t1!=null)
                    t1.setText(getContext().getString(R.string.demo_header_exampletitle1));

                TextView t2 = (TextView) view.findViewById(R.id.text_example2);
                if (t2!=null)
                    t2.setText(getContext().getString(R.string.demo_header_exampletitle2));
            }
        }
    }
```

Then you can add this custom `CustomHeaderInnerCard` to your `Card`:

``` java
        //Create a Card
        Card card = new Card(getActivity());

        //Create a CardHeader
        CustomHeaderInnerCard header = new CustomHeaderInnerCard(getContext());

        //Add Header to card
        card.addCardHeader(header);

        //Set card in the cardView
        CardView cardView = (CardView) getActivity().findViewById(R.id.card_header_inner);

        cardView.setCard(card);
```

![Screen](/demo/images/header/native/inner.png)

### Customizing the HeaderLayout

You can fully customize your header.

You can set your header layout, customizing card layout and inflating your header xml layout file.

First of all you have to provide your card layout.

The quickest way to start with this would be to copy the built-in layouts.

For **CardViewNative**:
 
clone [`res/layout/native_card_layout.xml`](/library-core/src/main/res/layout/native_card_layout.xml) or [`res/layout/native_card_thumbnail_layout.xml`](/library-core/src/main/res/layout/native_card_thumbnail_layout.xml)


You can see [`res/layout/carddemo_native_example_card1_layout.xml`](/demo/stock/src/main/res/layout/carddemo_native_example_card1_layout.xml).


``` xml
   <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
                 xmlns:card="http://schemas.android.com/apk/res-auto">

         <it.gmariotti.cardslib.library.view.component.CardHeaderView
            style="@style/card.native.header_outer_layout"
            android:id="@+id/card_header_layout"
            android:layout_width="match_parent"
            card:card_header_layout_resourceID="@layout/carddemo_example_card1_header_layout"
            android:layout_height="wrap_content"/>

   </LinearLayout>
```

For **CardView**:
 
clone [`res/layout/card_layout.xml`](/library-core/src/main/res/layout/card_layout.xml) or [`res/layout/card_thumbnail_layout.xml`](/library-core/src/main/res/layout/card_thumbnail_layout.xml)


You can see [`res/layout/carddemo_example_card1_layout.xml`](/demo/stock/src/main/res/layout/carddemo_example_card1_layout.xml).

``` xml
   <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
                 xmlns:card="http://schemas.android.com/apk/res-auto">

             
         <it.gmariotti.cardslib.library.view.component.CardHeaderView
             style="@style/card.header_outer_layout"
             android:id="@+id/card_header_layout"
             android:layout_width="match_parent"
             card:card_header_layout_resourceID="@layout/carddemo_example_card1_header_layout"
             android:layout_height="wrap_content"/>

   </LinearLayout>
```

Pay attention. The card header has a header (general) layout. You can specify it using this attr:`card:card_header_layout_resourceID="@layout/carddemo_example_card1_header_layout"`.

This layout is not the [header inner layout](#customizing-the-innercontent-header-layout) described above.

When you use a custom card layout with a custom header (general) layout is important to use the **same IDs** to preserve built-in features. When you use a custom inner layout you can change everything.

Then you have to define your header layout:

You can see an example in [`res/layout/carddemo_example_card1_header_layout.xml`](/demo/stock/src/main/res/layout/carddemo_example_card1_header_layout.xml)

``` xml
<!-- This is the Header Layout -->

<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_alignParentTop="true"
                android:layout_alignParentLeft="true"
                style="@style/card.native.header_compound_view">

    <!-- This is the Inner Content Header which you can populate runtime
         with setupInnerViewElements(android.view.ViewGroup, android.view.View) method in CardHeader class. -->
    <FrameLayout
        android:id="@+id/card_header_inner_frame"
        style="@style/card.native.header_inner_frame"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        />

</RelativeLayout>
```

Finally you can extend `CardHeader` and popolate your elements as above.

It is very important to preserve the element with `android:id="@+id/card_header_inner_frame"`.

Without this element, the `setupInnerViewElements` method in your `CardHeader` will not be called.

![Screen](/demo/images/header/native/layout.png)

In the CardViewNative override these dimens to change the margins:

```xml
    <dimen name="card_header_native_default_paddingLeft">0dp</dimen>
    <dimen name="card_header_native_default_paddingRight">0dp</dimen>
    <dimen name="card_header_native_default_paddingTop">0dp</dimen>
    <dimen name="card_header_native_default_paddingBottom">0dp</dimen>
```

![Screen](/demo/images/header/native/header_0.png)


### Style

You can customize some properties with your style and drawable files.
The quickest way to start with this would be to copy the specific style or drawable in your project and
change them.

For the **CardViewNative**:

These are the main **style properties**:

* `card.native.header_outer_layout`: header layout style
* `card.native.header_simple_title`: title style
* `card.native.header_button_frame`: button frame style
* `card.native.header_button_base`:  buttons style
* `card.native.header_button_base.overflow`: overflow button style
* `card.native.header_button_base.expand`: expand button style
* `card.native.header_button_base.other`: other button style

**margins**:

``` xml
    <dimen name="card_header_native_default_paddingLeft">12dp</dimen>
    <dimen name="card_header_native_default_paddingRight">12dp</dimen>
    <dimen name="card_header_native_default_paddingTop">12dp</dimen>
    <dimen name="card_header_native_default_paddingBottom">0dp</dimen>
``` 

**color properties**:

* `card_text_color_header`: default header text color

Example to change header title color:

``` xml
   <color name="card_text_color_header">#990066</color>
```

![Screen](/demo/images/header/title_color.png)

**drawable/selector properties**
 values/styles.xml

* [`card_menu_button_overflow_material`](/library-core/src/main/res/drawable/card_menu_button_overflow_material.xml): default selector used by overflow menu
* [`card_menu_button_expand_material_animator`](/library-core/src/main/res/drawable/card_menu_button_expand_material_animator.xml): default selector used by expand menu

``` xml
    <!-- Button Overflow in Header -->
    <style name="card.native.header_button_base.overflow" >
        <item name="android:background">@drawable/card_menu_button_overflow_material</item>
    </style>
    
    <!-- Button to Expand/Collapse in Header -->
    <style name="card.native.header_button_base.expand">
        <item name="android:background">@drawable/card_menu_button_expand_material</item>
    </style>
```

**drawable properties** 
values-v21/styles.xml

* [`card_menu_button_overflow_material`](/library-core/src/main/res/drawable-v21/card_menu_button_overflow_material.png): default image selector used by overflow menu (it has a ripple background)
* [`card_menu_button_expand_material_animator`](/library-core/src/main/res/drawable-v21/card_menu_button_expand_material_animator.xml): default image selector used by expand menu (it has a ripple background)

``` xml
    <!-- Button Overflow in Header -->
    <style name="card.native.header_button_base.overflow" >
        <item name="android:src">@drawable/card_menu_button_overflow_material</item>
    </style>

    <!-- Button to Expand/Collapse in Header -->
    <style name="card.native.header_button_base.expand">
        <item name="android:src">@drawable/card_menu_button_expand_material_animator</item>
    </style>
``` 

**Text Size** 
``` xml
    <dimen name="card_header_native_simple_title_text_size">18sp</dimen>
```

**Text Font**
values-v16/fonts.xml
``` xml
    <string name="card_native_font_fontFamily_header">sans-serif-medium</string>
``` 
values-v21/fonts.xml
``` xml
    <string name="card_native_font_fontFamily_header">sans-serif-medium</string>
```

For the **CardView**:

These are the main **style properties**:

* `card.header_outer_layout`: header layout style
* `card.header_simple_title`: title style
* `card.header_button_frame`: button frame style
* `card.header_button_base`:  buttons style
* `card.header_button_base.overflow`: overflow button style
* `card.header_button_base.expand`: expand button style
* `card.header_button_base.other`: other button style

**color properties**:

* `card_text_color_header`: default header text color

Example to change header title color:

``` xml
   <color name="card_text_color_header">#990066</color>
```

![Screen](/demo/images/header/title_color.png)

**drawable/selector properties** for android<21:

* `card_menu_button_rounded_overflow`: default selector used by overflow menu
* `card_menu_button_expand`: default selector used by expand menu

``` xml
    <!-- Button Overflow in Header -->
    <style name="card.header_button_base.overflow" >
        <item name="android:background">@drawable/card_menu_button_rounded_overflow</item>
    </style>
    <!-- Button to Expand/Collapse in Header -->
    <style name="card.header_button_base.expand">
        <item name="android:background">@drawable/card_menu_button_expand</item>
    </style>
``` 

**drawable properties** for android>=21:

* `ic_menu_overflow_card_rounded_dark_normal`: default image used by overflow menu (it has a ripple background)
* `ic_menu_expand_card_dark_normal`: default image selector used by expand menu (it has a ripple background)

``` xml
    <!-- Button Overflow in Header -->
    <style name="card.header_button_base.overflow" >
        <item name="android:src">@drawable/ic_menu_overflow_card_rounded_dark_normal</item>
    </style>

    <!-- Button to Expand/Collapse in Header -->
    <style name="card.header_button_base.expand">
        <item name="android:src">@drawable/ic_menu_expand_card_dark_normal</item>
    </style>

``` 


**Text Size** 
``` xml
    <dimen name="card_header_simple_title_text_size">20sp</dimen>
``` 

**Text Font**
values-v16/fonts.xml
``` xml
    <string name="card_font_fontFamily_header">sans-serif-condensed</string>
``` 
values-v21/fonts.xml
``` xml
    <string name="card_font_fontFamily_header">sans-serif-condensed</string>
```
Examples:

To change this button you have to override the styles described.


**Example to change pressed expand button**:

* you can copy ic_menu_expand_card_dark_pressed.png in your project and change it.
* or you can copy and modify the selector  card_menu_button_expand.xml
* or you can copy and modify the style `card.header_button_base.expand`

drawable/

``` xml
   <selector xmlns:android="http://schemas.android.com/apk/res/android">

       <item android:drawable="@drawable/ic_menu_expand_card_dark_red_pressed"
             android:state_pressed="true"/>
       <item android:drawable="@drawable/ic_menu_expand_card_dark_red_pressed"
             android:state_focused="true"/>
       <item android:drawable="@drawable/ic_menu_expand_card_dark_red_pressed"
             android:state_selected="true"/>
       <item android:drawable="@drawable/ic_menu_expand_card_dark_normal"/>
   </selector>
```

drawable-v21/ (remove the pressed state because it is provided by the ripple).
``` xml
    <item
        android:state_focused="true"
        android:drawable="@drawable/ic_menu_expand_card_dark_red_pressed" />
    <item
        android:drawable="@drawable/ic_menu_expand_card_dark_red_pressed"
        android:state_selected="true">

    </item>
    <item android:drawable="@drawable/ic_menu_expand_card_dark_normal"/>
```

![Screen](/demo/images/header/expand_red.png)
