# Customer Service Support Technician Tech Test
Test for CS Support candidates

## Requirements

Allowing for setting up the project, this tech test should take around 2 hours to complete.

1. Find the cause of issues in the application:
* There are three bugs to find
* You are not required to *fix* the bugs, your aim is to __identify__ what is causing them

2. Document your process:
  * What did you notice/find?
  * What are your recommendations?

## Getting Started
These instructions will get the application up and running on your local machine for bug finding purposes.

* Install Ruby [Ways of Installing Ruby](https://www.ruby-lang.org/en/downloads)
* Install Yarn [Ways of Installing Yarn](https://yarnpkg.com/lang/en/docs/install)
* Clone this git repository `git clone git@github.com:smartpension/smart-cs-support-test.git`
* cd into the locally cloned repo `smart-cs-support-test`
* Run `bin/setup`
* Start up your web-server (see the output of `bin/setup`)
 * If you get an error with: `getaddrinfo: nodename nor servname provided, or not known (SocketError)`
   * Try running your server with`./bin/rails server -b 0.0.0.0`

* Navigate to: http://localhost:3000

Remember to document __how__ you identified the bugs and attach your findings to your email back to us, have fun!!

____________________________________________________________________________________________________________________



    ## MY SOLUTIONS TO THE TEST


  1. When creating a company or simply trying to redirected to the company’s show page, the first company on the list will always show.

  * Before (Show method in the companies_controller):

     def show
      @company = Company.find(permitted_params[:id])
      @company = Company.first
    end

  * Solution:

    def show
        @company = Company.find(permitted_params[:id])
    end


    - I removed the second line which was overriding the first one. The second line was always showcasing the first company on the list.



  2. When creating a new employee an error message comes up that middle name/ surname cannot be blank.

    * Before (Employee model & Views Employees new.html.erb):

     <!--  class Employee < ApplicationRecord
        belongs_to :company
        validates :forename, :surname, :middlename, presence: true
      end -->


     <!--  # -->


     <!--  <div class="form-group">
        <%= form.label :forename %>
        <%= form.text_field :forename, class: "form-control" %>

        <p>
          <%= form.label :surname %>
          <%= form.text_field :middlename, class: "form-control" %>
        </p> -->


    * Solution:

      class Employee < ApplicationRecord
        belongs_to :company
        validates :forename, :surname, presence: true
      end

      <!-- # -->

      <div class="form-group">
        <%= form.label :forename %>
        <%= form.text_field :forename, class: "form-control" %>

        <p>
          <%= form.label :surname %>
          <%= form.text_field :surname, class: "form-control" %>
        </p>


      There are two ways to approach the matter:
      - Either you remove the middle name so its presence isn’t true, and add it in the form as an option
        (Not everybody has a middle name, but should be optional for those who have).

      - Or, as I have done in this example: that you remove the middle name from the model and from the form
        (Change surname/middle name to surname/surname). Like this we don’t get an error anymore.




    3. An error comes up when trying to edit an Employee & delete option is missing.

      * Before (views - companies show.html.erb):

         <!--  <tbody>
            <% for employee in @company.employees %>
              <tr>
                <td><%= employee.id %></td>
                <td><%= employee.forename %></td>
                <td><%= employee.surname %></td>
                <td><%= link_to "Edit", edit_company_employee_path(employee), class: "btn btn-primary btn-sm" %></td>
              </tr>
            <% end %>
          </tbody>
        </table> -->

      * Solution:

          <tbody>
            <% for employee in @company.employees %>
              <tr>
                <td><%= employee.id %></td>
                <td><%= employee.forename %></td>
                <td><%= employee.surname %></td>
                <td><%= link_to "Edit", edit_company_employee_path(@company, employee), class: "btn btn-primary btn-sm" %>
                <%= link_to "Destroy Employee", [@company, employee], method: :delete, data: { confirm: "Are you sure?" }, class: "btn btn-danger btn-sm" %></td>
              </tr>
            <% end %>
          </tbody>
        </table>


      - In the edit path I added @company because you need a company id to find an employee. In the routes you can also see that you need a company_id and an employee_id.
      - I also added a delete button because I had to go thru the edit then save, to then be able to delete an employee.




    <!-- Extra -->


    4. Listing Employees (views - Employees index.html.erb) - there is no button to go back to company show page unless you go back to home page.

      * Solution:

        <%= link_to "Create New Employee", new_company_employee_path(@company), class: "btn btn-primary" %>
        <%= link_to "Back to company", company_path(@company), class: "btn btn-primary"%>



    5. I noticed as well that the list of companies and employees don’t always start the counter with one when you delete one of the companies or employees. It is a small detail but its still noticeable.
        If it was me I would make a few changes in the view.

      * For example:

        <ul>
          <% @employees.each_with_index do |employee, index| %>
            <li><%= index + 1 %>. <%= employee.name %></li>
          <% end %>
        </ul>
