---
title: "Invitations"
excerpt: ""
---
## Create Invitation
[block:callout]
{
  "type": "warning",
  "title": "Sending e-mail with Invitations not working yet.",
  "body": "Errors may occur."
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "var syncano = new Syncano();\nsyncano.connect(\"our_API_key\");\nsyncano.setInstance(\"instanceName\");\n\nvar params = {\n\temail: \"adress@domain.com\",\n\trole: \"full\" // read / write / full\n};\n\nsyncano.Invitations.create(params).then(function () {\n\tconsole.log(\"Invitation created!\");\n}, function (err) {\n\tconsole.log(\"ERR: \" + err);\n});",
      "language": "javascript"
    },
    {
      "code": "import syncano\nfrom syncano.models import InstanceInvitation\n\nconnection = syncano.connect(api_key=\"API_KEY\")\n\ninvitation = InstanceInvitation.please.create(\n    email='EMAIL',\n    role='full',\n)",
      "language": "python",
      "name": "Python"
    }
  ]
}
[/block]
## List Invitations
[block:code]
{
  "codes": [
    {
      "code": "var syncano = new Syncano();\nsyncano.connect(\"our_API_key\");\nsyncano.setInstance(\"instanceName\");\n\nsyncano.Invitations.list().then(function (output) {\n\tconsole.log(output);\n}, function (err) {\n\tconsole.log(\"ERR: \" + err);\n});",
      "language": "javascript"
    },
    {
      "code": "import syncano\nfrom syncano.models import InstanceInvitation\n\nconnection = syncano.connect(api_key=\"API_KEY\")\n\nfor invitation in InstanceInvitation.please.all():\n    print(invitation.email, invitation.role)",
      "language": "python",
      "name": "Python"
    }
  ]
}
[/block]
## Get specified Invitation
[block:code]
{
  "codes": [
    {
      "code": "var syncano = new Syncano();\nsyncano.connect(\"our_API_key\");\nsyncano.setInstance(\"instanceName\");\n\nvar invitationId = 2;\n\nsyncano.Invitations.get(invitationId).then(function (output) {\n\tconsole.log(output);\n}, function (err) {\n\tconsole.log(\"ERR: \" + err);\n});",
      "language": "javascript"
    },
    {
      "code": "import syncano\nfrom syncano.models import InstanceInvitation\n\nconnection = syncano.connect(api_key=\"API_KEY\")\n\ninvitation = InstanceInvitation.please.get(id=INVITATION_ID)",
      "language": "python",
      "name": "Python"
    }
  ]
}
[/block]
## Remove Invitation
[block:code]
{
  "codes": [
    {
      "code": "var syncano = new Syncano();\nsyncano.connect(\"our_API_key\");\nsyncano.setInstance(\"instanceName\");\n\nvar invitationId = 2;\n\nsyncano.Invitations.remove(invitationId).then(function (output) {\n\tconsole.log(output);\n}, function (err) {\n\tconsole.log(\"ERR: \" + err);\n});",
      "language": "javascript"
    },
    {
      "code": "import syncano\nfrom syncano.models import InstanceInvitation\n\nconnection = syncano.connect(api_key=\"API_KEY\")\n\ninvitation = InstanceInvitation.please.get(id=INVITATION_ID)\ninvitation.delete()\n\n# or:\nInstanceInvitation.please.delete(id=INVITATION_ID)",
      "language": "python",
      "name": "Python"
    }
  ]
}
[/block]