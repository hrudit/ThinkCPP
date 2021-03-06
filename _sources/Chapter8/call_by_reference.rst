Call by reference
-----------------
.. index:: 
   single: call by reference

An alternative parameter-passing mechanism that is available in C++ is
called “pass by reference.” This mechanism makes it possible to pass a
structure to a procedure and modify it.

For example, you can reflect a point around the 45-degree line by
swapping the two coordinates. The most obvious (but incorrect) way to
write a ``reflect`` function is something like this:

::

   void reflect (Point p) {      // WRONG !!
     double temp = p.x;
     p.x = p.y;
     p.y = temp;
   }

But this won’t work, because the changes we make in ``reflect`` will
have no effect on the caller.

Instead, we have to specify that we want to pass the parameter by
reference. We do that by adding an ampersand (``&``) to the parameter
declaration:

::

   void reflect (Point& p) {
     double temp = p.x;
     p.x = p.y;
     p.y = temp;
   }

Now we can call the function in the usual way:

::

     printPoint (blank);
     reflect (blank);
     printPoint (blank);

.. activecode:: call_by_reference_AC_1
  :language: cpp

  Take a look at the active code below. ``reflect`` passes the parameter ``p``
  by reference. Notice that the output of this code matches what we expect it to be.
  ~~~~
  #include <iostream>
  using namespace std;

  struct Point {
      double x, y;
  };

  void reflect (Point& p) {
      double temp = p.x;
      p.x = p.y;
      p.y = temp;
  }

  void printPoint (Point p) {
      cout << "(" << p.x << ", " << p.y << ")" << endl;
  }

  int main() {
      Point blank = { 3.0, 4.0 };
      printPoint (blank);
      reflect (blank);
      printPoint (blank);
  }

::

   (3, 4)
   (4, 3)

Here's how we would draw a stack diagram for this program:

.. figure:: Images/8.7stackdiagram.png
   :scale: 50%
   :align: center
   :alt: image

The parameter ``p`` is a reference to the structure named ``blank``. The
usual representation for a reference is a dot with an arrow that points
to whatever the reference refers to.

The important thing to see in this diagram is that any changes that
``reflect`` makes in ``p`` will also affect ``blank``.

Passing structures by reference is more versatile than passing by value,
because the callee can modify the structure. It is also faster, because
the system does not have to copy the whole structure. On the other hand,
it is less safe, since it is harder to keep track of what gets modified
where. Nevertheless, in C++ programs, almost all structures are passed
by reference almost all the time. In this book I will follow that
convention.

.. fillintheblank:: call_by_reference_1

    Which symbol should you put in front of the data type of the parameter of a function to make it pass by reference?

    - :&: Correct!
      :.*: Try again!

.. mchoice:: call_by_reference_2
   :practice: T
   :answer_a: Passing structures by reference is more versatile
   :answer_b: Passing structures by reference is faster, because the system does not have to copy the whole structure
   :answer_c: In C++ programs, almost all structures are passed by reference almost all the time
   :answer_d: Passing structures by reference is is less safe, since it is harder to keep track of what gets modified where
   :correct: d
   :feedback_a: Try again!
   :feedback_b: Try again!
   :feedback_c: Try again!
   :feedback_d: Correct!

   Which is NOT a benefit to using pass by reference instead of pass by value?

.. mchoice:: call_by_reference_3
   :practice: T
   :answer_a: 2 4
   :answer_b: 2 4 2
   :answer_c: 4 4 2
   :answer_d: 2 4 4
   :correct: d
   :feedback_a: Take a look at exactly what is being outputted.
   :feedback_b: Remember the rules of pass by reference.
   :feedback_c: Take a look at exactly what is being outputted.
   :feedback_d: Correct!

   What will print?

   .. code-block:: cpp

      int addTwo(int& x) {
        cout << x << " ";
        x = x + 2;
        cout << x << " ";
        return x;
      }

      int main() {
        int num = 2;
        addTwo(num);
        cout << num << endl;
      }

.. mchoice:: call_by_reference_4
   :practice: T
   :answer_a: 6.0, 8.0, 3.0, 4.0
   :answer_b: 6.0, 8.0, 6.0, 8.0
   :answer_c: 6.08.03.04.0
   :answer_d: 6.08.06.08.0
   :correct: b
   :feedback_a: The ``&`` indicates pass by reference.
   :feedback_b: Correct!
   :feedback_c: The ``&`` indicates pass by reference.
   :feedback_d: Take a look at exactly what is being outputted.

   What will print?

   .. code-block:: cpp

      struct Point {
        double x, y;
      };

      void timesTwo (Point& p) {
        cout << "(" << p.x * 2 << ", " << p.y * 2 << ")";
      }

      int main() {
        Point blank = { 3.0, 4.0 };
        timesTwo (blank);
        cout << ", " << blank << endl;
      }
