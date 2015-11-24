---
layout: post
title: "Sending Gmail Emails With Rails"
date: 2015-11-23 21:59:02 -0500
comments: true
categories: "Flatiron&nbsp;School"
---
As part of our meetup project, I needed to figure out how to send email via Rails. Our website allows a user to select a favorite photo and then receive a link to that photo via email.

Rails sends email through something called the Action Mailer. The Rails Guides <a href="http://guides.rubyonrails.org/action_mailer_basics.html">explain</a> how to set up Action Mailer, but I needed to take some additional steps that the documentation didn't include. For instance, the guide explains how to send a welcome email after a user signs up, but we wanted to send an email later on - after the user selects a favorite photo. Here is how I got it working for Gmail.

As the guide points out, mailers are conceptually similar to controllers. We generate a mailer via the command line:

<pre>rails generate mailer UserMailer</pre>

This creates a file called <strong>app/mailers/user_mailer.rb</strong>. More on this file later.

In our project, the user enters an email address via a form. In the code below, the form_tag directs to the <strong>users/create</strong> action - the <strong>create</strong> method in the User controller - and the e-mail field passes an <strong>:email_address</strong> to the params. And since we want to send the user a photo URL, we pass the photo URL as a hidden field:

<pre><%= form_tag("users/create", method: "get") do %>
&lt;img src="<%=@picture_chosen.photo_url%>"&gt;
&lt;div&gt;
	<%= hidden_field_tag(:photo_url, @picture_chosen.photo_url) %>
	<%= email_field(:user, :email_address) %>
	<%= submit_tag("Submit")%>
<% end %>
&lt;/div&gt;</pre>

On submit, the data passes to the User controller's <strong>create</strong> method:

<pre>class UsersController < ApplicationController

	def create
	    @user = User.new(user_params)
	    respond_to do |format|
	    	@user.email_address = params[:user][:email_address]
	      @user.photo_url = params[:photo_url]
	      if @user.save
	        UserMailer.result_email(@user).deliver_now
	        format.html { redirect_to(root_path, notice: 'Favorite photo chosen') }
	        format.json { render json: root_path, status: :created, location: @user }
	      else
	        format.html { render action: 'new' }
	        format.json { render json: @user.errors, status: :unprocessable_entity }
	      end
	    end
	  end

	private

	def user_params
		params.require(:user).permit(user: [:email_address, :photo_url] )
	end

	end</pre>

The email address gets saved as <strong>@user.email_address</strong>, which we must add to the permitted user params. Upon <strong>@user.save</strong>, we get directed to:

	UserMailer.result_email(@user).deliver_now

UserMailer is the mailer we created at the beginning via <strong>rails generate mailer UserMailer</strong>. (The <strong>deliver_now</strong> at the end sends the email immediately; there is also an option for <strong>deliver_later</strong>.)

So we're directed to the UserMailer, and we need to put the following code in there:

	class UserMailer < ApplicationMailer
		def result_email(user)
			@user = user
			@url  = user.photo_url
	    mail(to: @user.email_address, subject: 'Your favorite photo')
		end
	end

We're setting the individual user and the photo URL as objects so we can pass them, and the <strong>mail</strong> command specifies the email address and the email subject.

The UserMailer inherits from ApplicationMailer, which, like UserMailer, lives in the app/mailers/ folder. ApplicationMailer needs to contain the email address from which the email will be sent:

		class ApplicationMailer < ActionMailer::Base
		  default from: "email_sender@gmail.com"
		  layout 'mailer'
		end

(Replace "email_sender" with the actual sending account.)

To go along with the UserMailer, there is a corresponding user_mailer folder under app/views/. The user_mailer folder contains two files, <strong>result_email.html.erb</strong> and <strong>result_email.text.erb</strong>. The former sends an HTML-formatted email; the latter contains a text-formatted email, which will be sent if the user's email client doesn't accept HTML-formatted email.

Here's our HTML-formatted email in our <strong>result_email.html.erb</strong> file:

<pre>&lt;!DOCTYPE html&gt;
&lt;html&gt;
  &lt;head&gt;
    &lt;meta content='text/html; charset=UTF-8' http-equiv='Content-Type' /&gt;
  &lt;/head&gt;
  &lt;body&gt;
    &lt;h1&gt;Hello!&lt;/h1&gt;
    &lt;p&gt;
      Thanks for picking your favorite photo! Here is a link to the photo you chose as your favorite:
    &lt;/p&gt;
  	&lt;p&gt;
  	  <%= @url %>
  	&lt;/p&gt;
  &lt;/body&gt;
&lt;/html&gt;</pre>


Note that since we created a @url object in the UserMailer, we can call it in the email above.

This sets everything up, but the email won't actually get sent unless we add the following code to the three files under config/environments/ -- development.rb, production.rb, and test.rb:

		config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }
		config.action_mailer.delivery_method = :smtp
		config.action_mailer.raise_delivery_errors = true
		config.action_mailer.smtp_settings =  {
		  :address    => "smtp.gmail.com",
		  :port       => 587,
		  :domain     => 'localhost',
		  :user_name  => ENV["gmail_username"],
		  :password   => ENV["gmail_password"],
		  :authentication   => :login,
		  :enable_starttls_auto   => true
		  }

This works for Gmail. Other email clients will require slightly different settings.

Note that <strong>:user_name</strong> and <strong>:password</strong> are ENV keys, because these are the sender's username and password, and we don't want the password to be visible to others. We should also install the <a href="https://github.com/laserlemon/figaro">Figaro gem</a> and require it in the Gemfile. This creates a file called config/application.yml, which gets added to the .gitignore file and will keep the email name and password hidden. Open this file and add the following (replacing the sample user name and password with the real ones):

<pre>gmail_username: "emailer_sender@gmail.com"
gmail_password: "123456789"</pre>

Make sure "gmail_username" and "gmail_password" are spelled the same in both the config/environments files and the config/application.yml file.

And we're done. Your application can now send email.